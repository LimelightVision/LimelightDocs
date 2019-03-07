Case Study: DEEP SPACE 2019 Examples
====================================

.. Summary

1. Example programs in C++, Java and Labview for using a limelight camera to drive up to a goal in Deep Space.

The 2019 FRC game Deep Space has vision targets above many of the goals that you need to drive up to.  Below you can find complete example programs in C++, Java, and Labview which implement a simple method for automatically driving a differential drive robot to a goal in Deep Space.

These are very simple programs and only meant to show the concept of using limelight tracking data to control your robot.  In each program, you can drive your robot with a gamepad.  If you hold the 'A' button down, and the limelight sees a valid target (depending on the settings in your pipeline) then the robot will automatically drive towards the target.  Be careful to tune the various constants in the code for your particular robot.  Some robots turn or drive more easily than others so tuning the proportional control constants must be done on a case-by-case basis.  Also make sure the robot drives correctly using the gamepad controller before enabling the limelight seeking behavior.  You may need to invert some of your motor controllers.


.. tabs::
	
	.. tab:: Java

		.. code-block:: java

			package frc.robot;

			import edu.wpi.first.wpilibj.TimedRobot;
			import edu.wpi.first.wpilibj.smartdashboard.SendableChooser;
			import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
			import edu.wpi.first.wpilibj.VictorSP;
			import edu.wpi.first.wpilibj.SpeedControllerGroup;
			import edu.wpi.first.wpilibj.XboxController;
			import edu.wpi.first.wpilibj.GenericHID.Hand;
			import edu.wpi.first.wpilibj.drive.DifferentialDrive;
			import edu.wpi.first.networktables.*;

			/**
			 * The VM is configured to automatically run this class, and to call the
			 * functions corresponding to each mode, as described in the TimedRobot
			 * documentation. If you change the name of this class or the package after
			 * creating this project, you must also update the build.gradle file in the
			 * project.
			 */
			public class Robot extends TimedRobot {

			  private static final String kDefaultAuto = "Default";
			  private static final String kCustomAuto = "My Auto";
			  private String m_autoSelected;
			  private final SendableChooser<String> m_chooser = new SendableChooser<>();

			  private VictorSP m_Left0 = new VictorSP(0);
			  private VictorSP m_Left1 = new VictorSP(1);
			  private VictorSP m_Right0 = new VictorSP(2);
			  private VictorSP m_Right1 = new VictorSP(3);
			  private SpeedControllerGroup m_LeftMotors = new SpeedControllerGroup(m_Left0,m_Left1);
			  private SpeedControllerGroup m_RightMotors = new SpeedControllerGroup(m_Right0,m_Right1);
			  private DifferentialDrive m_Drive = new DifferentialDrive(m_LeftMotors,m_RightMotors);

			  private XboxController m_Controller = new XboxController(0);

			  private boolean m_LimelightHasValidTarget = false;
			  private double m_LimelightDriveCommand = 0.0;
			  private double m_LimelightSteerCommand = 0.0;

			  /**
			   * This function is run when the robot is first started up and should be
			   * used for any initialization code.
			   */
			  @Override
			  public void robotInit() {
				m_chooser.setDefaultOption("Default Auto", kDefaultAuto);
				m_chooser.addOption("My Auto", kCustomAuto);
				SmartDashboard.putData("Auto choices", m_chooser);
			  }

			  /**
			   * This function is called every robot packet, no matter the mode. Use
			   * this for items like diagnostics that you want ran during disabled,
			   * autonomous, teleoperated and test.
			   *
			   * <p>This runs after the mode specific periodic functions, but before
			   * LiveWindow and SmartDashboard integrated updating.
			   */
			  @Override
			  public void robotPeriodic() {
			  }

			  /**
			   * This autonomous (along with the chooser code above) shows how to select
			   * between different autonomous modes using the dashboard. The sendable
			   * chooser code works with the Java SmartDashboard. If you prefer the
			   * LabVIEW Dashboard, remove all of the chooser code and uncomment the
			   * getString line to get the auto name from the text box below the Gyro
			   *
			   * <p>You can add additional auto modes by adding additional comparisons to
			   * the switch structure below with additional strings. If using the
			   * SendableChooser make sure to add them to the chooser code above as well.
			   */
			  @Override
			  public void autonomousInit() {
				m_autoSelected = m_chooser.getSelected();
			  }

			  /**
			   * This function is called periodically during autonomous.
			   */
			  @Override
			  public void autonomousPeriodic() {
			  }

			  /**
			   * This function is called periodically during operator control.
			   */
			  @Override
			  public void teleopPeriodic() {

				Update_Limelight_Tracking();

				double steer = m_Controller.getX(Hand.kRight);
				double drive = -m_Controller.getY(Hand.kLeft);
				boolean auto = m_Controller.getAButton();

				steer *= 0.70;
				drive *= 0.70;

				if (auto)
				{
				  if (m_LimelightHasValidTarget)
				  {
					m_Drive.arcadeDrive(m_LimelightDriveCommand,m_LimelightSteerCommand);
				  }
				  else
				  {
					m_Drive.arcadeDrive(0.0,0.0);
				  }
				}
				else
				{
				  m_Drive.arcadeDrive(drive,steer);
				}
			  }

			  @Override
			  public void testPeriodic() {
			  }

			  /**
			   * This function implements a simple method of generating driving and steering commands
			   * based on the tracking data from a limelight camera.
			   */
			  public void Update_Limelight_Tracking()
			  {
				// These numbers must be tuned for your Robot!  Be careful!
				final double STEER_K = 0.03;  			// how hard to turn toward the target
				final double DRIVE_K = 0.26;			// how hard to drive fwd toward the target
				final double DESIRED_TARGET_AREA = 13.0;	// Area of the target when the robot reaches the wall
				final double MAX_DRIVE = 0.7;			// Simple speed limit so we don't drive too fast

				double tv = NetworkTableInstance.getDefault().getTable("limelight").getEntry("tv").getDouble(0);
				double tx = NetworkTableInstance.getDefault().getTable("limelight").getEntry("tx").getDouble(0);
				double ty = NetworkTableInstance.getDefault().getTable("limelight").getEntry("ty").getDouble(0);
				double ta = NetworkTableInstance.getDefault().getTable("limelight").getEntry("ta").getDouble(0);
				
				if (tv < 1.0)
				{
				  m_LimelightHasValidTarget = false;
				  m_LimelightDriveCommand = 0.0;
				  m_LimelightSteerCommand = 0.0;
				  return;
				}
				
				m_LimelightHasValidTarget = true;

				// Start with proportional steering
				double steer_cmd = tx * STEER_K;
				m_LimelightSteerCommand = steer_cmd;

				// try to drive forward until the target area reaches our desired area
				double drive_cmd = (DESIRED_TARGET_AREA - ta) * DRIVE_K;

				// don't let the robot drive too fast into the goal
				if (drive_cmd > MAX_DRIVE)
				{
				  drive_cmd = MAX_DRIVE;
				}
				m_LimelightDriveCommand = drive_cmd;
			  }
			}
		
		
	.. tab:: C++ Header
	
		.. code-block:: c++

			#pragma once

			#include <string>

			#include <frc/TimedRobot.h>
			#include <frc/smartdashboard/SendableChooser.h>
			#include <frc/VictorSP.h>
			#include <frc/SpeedControllerGroup.h>
			#include <frc/XboxController.h>
			#include <frc/drive/DifferentialDrive.h>


			class Robot : public frc::TimedRobot {
			 public:
			  Robot();
			  
			  void RobotInit() override;
			  void RobotPeriodic() override;
			  void AutonomousInit() override;
			  void AutonomousPeriodic() override;
			  void TeleopInit() override;
			  void TeleopPeriodic() override;
			  void TestPeriodic() override;

			  void Update_Limelight_Tracking();

			 private:
			  frc::SendableChooser<std::string> m_chooser;
			  const std::string kAutoNameDefault = "Default";
			  const std::string kAutoNameCustom = "My Auto";
			  std::string m_autoSelected;

			  frc::VictorSP m_Left0;
			  frc::VictorSP m_Left1;
			  frc::VictorSP m_Right0;
			  frc::VictorSP m_Right1;

			  frc::SpeedControllerGroup m_LeftMotors { m_Left0,m_Left1 };
			  frc::SpeedControllerGroup m_RightMotors { m_Right0,m_Right1 };
			  frc::DifferentialDrive m_Drive{ m_LeftMotors, m_RightMotors };
			  frc::XboxController m_Controller;

			  bool m_LimelightHasTarget;
			  double m_LimelightTurnCmd;
			  double m_LimelightDriveCmd;
			};

	.. tab:: C++ Source

		.. code-block:: c++

	
			#include "Robot.h"
			#include <iostream>
			#include <frc/smartdashboard/SmartDashboard.h>
			#include <networktables/NetworkTable.h>
			#include <networktables/NetworkTableInstance.h>


			double clamp(double in,double minval,double maxval)
			{
			  if (in > maxval) return maxval;
			  if (in < minval) return minval;
			  return in;
			}


			Robot::Robot() :
			  m_Left0(0),
			  m_Left1(1),
			  m_Right0(2),
			  m_Right1(3),
			  m_Controller(0)
			{
			  m_LeftMotors.SetInverted(false);
			  m_RightMotors.SetInverted(false);
			}

			 
			void Robot::RobotInit() {
			  m_chooser.SetDefaultOption(kAutoNameDefault, kAutoNameDefault);
			  m_chooser.AddOption(kAutoNameCustom, kAutoNameCustom);
			  frc::SmartDashboard::PutData("Auto Modes", &m_chooser);
			}

			void Robot::RobotPeriodic() {}

			void Robot::AutonomousInit() {}

			void Robot::AutonomousPeriodic() {}

			void Robot::TeleopInit() {}

			void Robot::TeleopPeriodic() 
			{
			  Update_Limelight_Tracking();
			  
			  bool do_limelight = m_Controller.GetAButton();
			  if (do_limelight)
			  {
				if (m_LimelightHasTarget)
				{
				  m_Drive.ArcadeDrive(m_LimelightDriveCmd,m_LimelightTurnCmd);
				}
				else
				{
				  m_Drive.ArcadeDrive(0.0,0.0);
				}
			  }
			  else
			  {
				// Tank Drive
				//double left = -m_Controller.GetY(frc::GenericHID::JoystickHand::kLeftHand);
				//double right = -m_Controller.GetY(frc::GenericHID::JoystickHand::kRightHand);
				//m_Drive.TankDrive(left,right);

				// Arcade Drive
				double fwd = -m_Controller.GetY(frc::GenericHID::JoystickHand::kLeftHand);
				double turn = m_Controller.GetX(frc::GenericHID::JoystickHand::kRightHand); 
				fwd *= 0.7f;
				turn *= 0.7f;
				m_Drive.ArcadeDrive(fwd,turn);
			  }
			}

			void Robot::TestPeriodic() {}

			void Robot::Update_Limelight_Tracking()
			{
			  // Proportional Steering Constant:
			  // If your robot doesn't turn fast enough toward the target, make this number bigger
			  // If your robot oscillates (swings back and forth past the target) make this smaller
			  const double STEER_K = 0.05;

			  // Proportional Drive constant: bigger = faster drive
			  const double DRIVE_K = 0.26;

			  // Area of the target when your robot has reached the goal
			  const double DESIRED_TARGET_AREA = 13.0;
			  const double MAX_DRIVE = 0.65;
			  const double MAX_STEER = 1.0f;

			  std::shared_ptr<NetworkTable> table = nt::NetworkTableInstance::GetDefault().GetTable("limelight");
			  double tx = table->GetNumber("tx",0.0);
			  double ty = table->GetNumber("ty",0.0);
			  double ta = table->GetNumber("ta",0.0);
			  double tv = table->GetNumber("tv",0.0);

			  if (tv < 1.0)
			  {
				m_LimelightHasTarget = false;
				m_LimelightDriveCmd = 0.0;
				m_LimelightTurnCmd = 0.0;
			  }
			  else
			  {
				m_LimelightHasTarget = true;

				// Proportional steering
				m_LimelightTurnCmd = tx*STEER_K;
				m_LimelightTurnCmd = clamp(m_LimelightTurnCmd,-MAX_STEER,MAX_STEER);

				// drive forward until the target area reaches our desired area
				m_LimelightDriveCmd = (DESIRED_TARGET_AREA - ta) * DRIVE_K;
				m_LimelightDriveCmd = clamp(m_LimelightDriveCmd,-MAX_DRIVE,MAX_DRIVE);
			  }
			}


			#ifndef RUNNING_FRC_TESTS
			int main() { return frc::StartRobot<Robot>(); }
			#endif


	.. tab:: LabView

		Here is a block diagram for a LabView VI which reads tracking data from a Limelight and generates drive and steer commands.  This image is a "LabView Snippet".  Just save the image file to your computer and then drag it into a labview VI and the block diagram will be reproduced.
		
		.. image:: img/Labview_Tank_VI.png
		
		
		You can also download the entire labview source code from this link:
		
		<https://www.mediafire.com/file/f35w3wllbmj9yt7/DeepSpaceLabviewExample.zip/file/>`_ 


