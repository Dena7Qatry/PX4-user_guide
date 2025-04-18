# 集成测试

This topic explains how to run (and extend) PX4's ROS-based integration tests.

:::note
[MAVSDK Integration Testing](../test_and_ci/integration_testing_mavsdk.md) is preferred when writing new tests. Use the ROS-based integration test framework for use cases that *require* ROS (e.g. object avoidance).

All PX4 integraton tests are executed automatically by our [Continuous Integration](../test_and_ci/continous_integration.md) system.
:::

## ROS / MAVROS 测试

* [jMAVSim 仿真模拟](../simulation/jmavsim.md)
* [Gazebo Classic Simulator](../sim_gazebo_classic/README.md)
* [ROS 和 MAVROS](../simulation/ros_interface.md)

## Execute Tests

To run the MAVROS tests:

```sh
cd <Firmware_clone>
source integrationtests/setup_gazebo_ros.bash $(pwd)
rostest px4 mavros_posix_tests_iris.launch
```

`test_target` is a makefile targets from the set: *tests_mission*, *tests_mission_coverage*, *tests_offboard* and *tests_avoidance*.

Test can also be executed directly by running the test scripts, located under `test/`:
```sh
rostest px4 mavros_posix_tests_iris.launch gui:=true headless:=false
```

在 `launch/mavros_posix_tests_irisl.launch` 中添加测试组中的新条目：
```sh
./test/rostest_px4_run.sh mavros_posix_tests_offboard_posctl.test
```

如上所述运行完整的测试套件。

```sh
# 开始仿真
cd <Firmware_clone>
source integrationtests/setup_gazebo_ros.bash $(pwd)
roslaunch px4 mavros_posix_sitl.launch

# 运行测试（在新的 shell 中）：
cd <Firmware_clone>
source integrationtests/setup_gazebo_ros.bash $(pwd)
rosrun px4 mavros_new_test.py
```

The **.test** files launch the corresponding Python tests defined in `integrationtests/python_src/px4_it/mavros/`


## Write a New MAVROS Test (Python)

This section explains how to write a new python test using ROS(1)/MAVROS, test it, and add it to the PX4 test suite.

We recommend you review the existing tests as examples/inspiration ([integrationtests/python_src/px4_it/mavros/](https://github.com/PX4/PX4-Autopilot/tree/main/integrationtests/python_src/px4_it/mavros)). The official ROS documentation also contains information on how to use [unittest](http://wiki.ros.org/unittest) (on which this test suite is based).

To write a new test:

1. Create a new test script by copying the empty test skeleton below:
    ```python
    #!/usr/bin/env python
    # [... LICENSE ...]

    #
    # @author Example Author <author@example.com>
    #
    PKG = 'px4'

    import unittest
    import rospy
    import rosbag

    from sensor_msgs.msg import NavSatFix

    class MavrosNewTest(unittest.TestCase):
        """
        Test description
        """

        def setUp(self):
            rospy.init_node('test_node', anonymous=True)
            rospy.wait_for_service('mavros/cmd/arming', 30)

            rospy.Subscriber("mavros/global_position/global", NavSatFix, self.global_position_callback)
            self.rate = rospy.Rate(10) # 10hz
            self.has_global_pos = False

        def tearDown(self):
            pass

        #
        # General callback functions used in tests
        #
        def global_position_callback(self, data):
            self.has_global_pos = True

        def test_method(self):
            """Test method description"""

            # FIXME: hack to wait for simulation to be ready
            while not self.has_global_pos:
                self.rate.sleep()

            # TODO: execute test

    if __name__ == '__main__':
        import rostest
        rostest.rosrun(PKG, 'mavros_new_test', MavrosNewTest)
    ```

1. Run the new test only
   - Start the simulator:
        ```sh
        cd <PX4-Autopilot_clone>
        source Tools/simulation/gazebo/setup_gazebo.bash
        roslaunch launch/mavros_posix_sitl.launch
        ```
    - Run test (in a new shell):
        ```
        cd <PX4-Autopilot_clone>
        source Tools/simulation/gazebo/setup_gazebo.bash
        rosrun px4 mavros_new_test.py
        ```

1. Add new test node to a launch file

   - In `test/` create a new `<test_name>.test` ROS launch file.
   - Call the test file using one of the base scripts *rostest_px4_run.sh* or *rostest_avoidance_run.sh*

1. (Optional) Create a new target in the Makefile
   - Open the Makefile
   - Search the *Testing* section
   - Add a new target name and call the test

   For example:
    ```sh
    tests_<new_test_target_name>: rostest
        @"$(SRC_DIR)"/test/rostest_px4_run.sh mavros_posix_tests_<new_test>.test
    ```

Run the tests as described above.
