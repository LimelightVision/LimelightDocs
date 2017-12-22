Case Study: Aiming Using Vision
===============================

.. Summary
1. You can aim your robot using only a limelight and your drivetrain.
2. We added the limelight to a robot, implemented the code and tuned it in 1hr.

Using high-framerate vision tracking, it is now possible to use the vision pipeline directly as the “sensor” in a PID control loop to guide your robot or turret.  In order to test this idea we added a limelight to a couple of our older robots and made them aim at vision targets using nothing more than their drivetrain and the networks table data being reported by the limelight. 

Our first test candidate was our 2017 robot which uses a wide, 6-wheel drivetran with colson wheels.  Here is a picture of us adding a limelight onto the robot in order to do this test:

.. image:: img/CS_aim_limelight_mounted.JPG

Next we added some code to the robot which would run whenever the driver holds a button on the joystick.  We added a block of code like this into our operator control function:

.. Code:: 
If (joystick->GetRawButton(9))
{
        Tx = networktables->Get_TX
        Steering_adjust = -0.1f * tx;
        Left_command+=steering_adjust;
        Right_command-=steering_adjust;
}

Right out of the box, this mostly worked.  The robot turns in the direction of the target automatically whenever you are holding the button.  If you move the target around, it turns to follow it.  However, using the live video feed on the dashboard, we could see that there was one big problem:  The robot wasn't always making it all the way to perfectly align with the target.  In some games with small targets, (like 2016 and 2017) this wouldn’t be good enough.

There are a few ways to approach this problem.  What we have implemented so far is a simple proportional control loop.  We calculated the error in heading and multiplied that by a constant, thus making a motor command which is proportional to the error.  As the error goes to zero, our command will go to zero.  The problem is that there is a lot of friction involved when the robot tries to turn.  Very small commands will not turn the robot at all.  At small angles, the command can become too small to actually move the robot.  You might find that your robot reaches its target well when you start out with a large targeting error but it just can’t aim at all if you start out really close.  

There are a few ways to solve this problem but here is a really simple solution.  We used a concept the “minimum command”.  If the error is bigger than some threshhold, just add a constant to your motor command which roughly represents the minimum amount of power needed for the robot to actually move (you actually want to use a little bit less than this).  The new code looks like this:

.. Code::
If (joystick->GetRawButton(9))
{
	float steering_adjust = 0.0f;
    Tx = networktables->Get_TX
    If (tx > 1.0)
	{
		steering_adjust = -Kp*tx - min_command;
	}
	else if (tx < 1.0)
	{
        Steering_adjust = -Kp*tx + min_command;
	}
	Left_command += steering_adjust;
	Right_command -= steering_adjust;
}

A little tuning on Kp and min_command should get your robot aiming directly at the target very quickly.  You want to use the smallest possible min_command; if you set this term too high your robot will oscillate as it overshoots the target.

