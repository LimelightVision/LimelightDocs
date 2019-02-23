Case Study: 2017 Fuel Robot 
===============================

.. Summary

1. Entire c++ robot program which implements 2017 fuel scoring.

In this example, we present a complete robot program which implements the 2017 boiler goal.  We tested this program on the 987 steamworks robot and were able to easily score fuel in the boiler.  For this example we aim and move into range using the robot's drivetrain.  One thing that was immediately apparent to us is that limelight let us implement this feature in a tiny fraction of the amount of code and time compared to our real 2017 robot.  

There are also a couple of other features in the code below such as the ability to blink the LEDs and dynamically toggle between multiple vision pipelines.  This example also shows how to use a Talon SRX speed controller along with a magnetic encoder to control the RPM of the shooter wheel.

.. code-block:: c++

	#include <iostream>
	#include <string>

	#include <Drive/DifferentialDrive.h>
	#include <Joystick.h>
	#include <SampleRobot.h>
	#include <SmartDashboard/SendableChooser.h>
	#include <SmartDashboard/SmartDashboard.h>
	#include <Spark.h>
	#include <Victor.h>
	#include <Timer.h>
	#include "ctre/phoenix/MotorControl/CAN/TalonSRX.h"

	//
	#define Vic_Drive_Left_1 0
	#define Vic_Drive_Left_2 1
	#define Vic_Drive_Right_1 2
	#define Vic_Drive_Right_2 3

	#define Tal_Shooter_Wheel 21
	#define Tal_Shooter_Wheel_2 2
	#define Tal_Intake 3
	#define Tal_Uptake 9

	class Robot : public frc::SampleRobot 
	{
	public:
	
	Robot() :
		LeftDrive(Vic_Drive_Left_1),
		RightDrive(Vic_Drive_Right_1),
		LeftDrive2(Vic_Drive_Left_2),
		RightDrive2(Vic_Drive_Right_2),
		Shooter(Tal_Shooter_Wheel),
		Shooter_2(Tal_Shooter_Wheel_2),
		Intake(Tal_Intake),
		Uptake(Tal_Uptake)
	{
		// This code sets up two Talons to work together and implement PID RPM control
		Shooter.SetControlMode(CTRE::MotorControl::ControlMode::kSpeed);
		Shooter_2.SetControlMode(CTRE::MotorControl::ControlMode::kFollower);
		Shooter_2.Set(Tal_Shooter_Wheel);
		Shooter.SetClosedLoopOutputDirection(false);
		Shooter.SetStatusFrameRateMs(CTRE::MotorControl::CAN::TalonSRX::StatusFrameRateAnalogTempVbat,10);

		Shooter.SetFeedbackDevice(CTRE::MotorControl::CAN::TalonSRX::CtreMagEncoder_Relative);
		SetShooterConstants(.0002f, .00009f, .0000f, .000006f);  // P,I,D,F constants for the shooter wheel 
		Shooter.SetAllowableClosedLoopErr(10);
		Shooter.ConfigNominalOutputVoltage(+0.f,-0.f);
		Shooter.ConfigPeakOutputVoltage(+12.f,-12.f);
		Shooter.SelectProfileSlot(0);
		Shooter.SetSensorDirection(true);
		Shooter.SetVoltageRampRate(100);
		Shooter.SetIzone(300);
		Shooter_2.SetVoltageRampRate(100);
	}

	void RobotInit()
	{
	}

	void Autonomous()
	{
	}

	/*
	 * Operator Control
	 */
	void OperatorControl() override
	{
		while (IsOperatorControl() && IsEnabled())
		{
			// Read the joysticks!
			float left_command = m_stickLeft.GetY();
			float right_command = m_stickRight.GetY();

			// Get limelight table for reading tracking data
			std::shared_ptr<NetworkTable> table = NetworkTable::GetTable("limelight");

			float KpAim = 0.045;
			float KpDist = 0.0f; //0.09;
			float AimMinCmd = 0.095f;

			float targetX = table->GetNumber("tx", 0);
			float targetY = table->GetNumber("ty", 0);
			float targetA = table->GetNumber("ta", 0);
			
			// Aim error and distance error based on calibrated limelight cross-hair
			float aim_error = targetX;
			float dist_error = targetY;

			// Steering adjust with a 0.2 degree deadband (close enough at 0.2deg)
			float steeringAdjust = KpAim*aim_error;
			if (targetX > .2f) steeringAdjust += AimMinCmd;
			else if (targetX < -.2f) steeringAdjust -= AimMinCmd;

			// Distance adjust, drive to the correct distance from the goal
			float drivingAdjust = KpDist*dist_error;
			bool doTarget = false;

			if(m_stickLeft.GetRawButton(3)) 	// Aim using pipeline 0
			{
				doTarget = true;
				table->PutNumber("pipeline", 0);
			}
			else if (m_stickLeft.GetRawButton(2))	// Aim using pipeline 1
			{
				doTarget = true;
				table->PutNumber("pipeline", 1);
			}

			if(doTarget)				// If auto-aiming, adjust drive and steer
			{
				ShooterSetRPM(3300);
				left_command += drivingAdjust - steeringAdjust;
				right_command += drivingAdjust + steeringAdjust;
			}
			else
			{
				ShooterOff();
			}

			// Tank drive, send left and right drivetrain motor commands
			StandardTank(left_command,right_command);

			if(m_stickRight.GetRawButton(3))  	// Suck in and shoot balls
			{
				IntakeIn();
				UptakeUp();
			}
			else if(m_stickRight.GetRawButton(2))	// Spit out balls
			{
				IntakeIn();
				UptakeDown();
			}
			else					// Leave the balls alone!
			{
				IntakeOff();
				UptakeOff();
			}
			if(m_stickLeft.GetRawButton(5))		// Joystick Button 5 = Flash the LEDs
			{
				table->PutNumber("ledMode", 2); //flash the lights
			}
			else
			{
				table->PutNumber("ledMode", 0); //turn the lights on
			}

			// wait for a motor update time
			frc::Wait(0.005);
		}
	}

	/*
	 * Runs during test mode
	 */
	void Test() override {}


	void StandardTank(float left, float right)
	{
		LeftDrive.Set(-left);
		LeftDrive2.Set(-left);
		RightDrive.Set(right);
		RightDrive2.Set(right);
	}

	//
	// Shooter Functions - uses talon PID to control shooter wheel RPM
	// Set the P,I,D,F constants in the Talon, these values depend heavily on your mechanism
	//
	void SetShooterConstants(float p,float i,float d,float f)
	{
		p *= 1024.f;
		i *= 1024.f;
		d *= 1024.f;
		f *= 1024.f;

		Shooter.SetP(p);
		Shooter.SetI(i);
		Shooter.SetD(d);
		Shooter.SetF(f);
	}

	//
	// Tell the talons our desired shooter wheel RPM
	//
	void ShooterSetRPM(float desrpm)
	{
		Shooter.SetControlMode(CTRE::MotorControl::ControlMode::kSpeed);
		Shooter_2.SetControlMode(CTRE::MotorControl::ControlMode::kFollower);
		Shooter_2.Set(Tal_Shooter_Wheel);
		Shooter.Set(desrpm);
	}

	// 
	// Just set the power -1..+1, not currently using this now that RPM control is set up
	//
	void ShooterSetPower(float power)
	{
		Shooter.SetControlMode(CTRE::MotorControl::ControlMode::kPercentVbus);
		Shooter_2.SetControlMode(CTRE::MotorControl::ControlMode::kPercentVbus);
		Shooter_2.Set(power);
		Shooter.Set(power);
	}

	//
	// Turn off the shooter wheel
	//
	void ShooterOff()
	{
		Shooter.SetControlMode(CTRE::MotorControl::ControlMode::kPercentVbus);
		Shooter_2.SetControlMode(CTRE::MotorControl::ControlMode::kPercentVbus);
		Shooter.Set(0.0f);
		Shooter_2.Set(0.0f);
	}

	//
	// Intake Functions
	//
	void IntakeIn()
	{
		Intake.Set(-.8f);
	}
	void IntakeOut()
	{
		Intake.Set(.8f);
	}
	void IntakeShooting()
	{
		Intake.Set(-1.0f);
	}

	void IntakeOff()
	{
		Intake.Set(0);
	}

	//
	// Uptake Functions
	//
	void UptakeUp()
	{
		Uptake.Set(-1.0f);
	}
	void UptakeDown()
	{
		Uptake.Set(1.0f);
	}
	void UptakeOff()
	{
		Uptake.Set(0);
	}

	private:

		// Robot drive system
		frc::Victor LeftDrive;
		frc::Victor RightDrive;
		frc::Victor LeftDrive2;
		frc::Victor RightDrive2;

		// shooter wheel
		CTRE::MotorControl::CAN::TalonSRX Shooter;
		CTRE::MotorControl::CAN::TalonSRX Shooter_2;
		CTRE::MotorControl::CAN::TalonSRX Intake;
		CTRE::MotorControl::CAN::TalonSRX Uptake;

		// Joystick inputs
		frc::Joystick m_stickLeft{0};
		frc::Joystick m_stickRight{1};
	};

	START_ROBOT_CLASS(Robot)
