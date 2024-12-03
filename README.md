# MAPVIZ-Ros-Noetic + Using UBLOX F9P-GPS(or any other)

Hey this is a small guide for setting up mapviz in ros-noetic and combing it to use with a GPS, the GPS I have used is UBLOX F9P.

## Steps for installing Mapviz in ROS Noetic

Clone the **Master** branch of the Swri-Mapviz repository in your ROS Workspace be careful that the the default one clones the ros-2 devlopment repository.
Visit this git page: https://github.com/swri-robotics/mapviz/tree/master or use the command below once you change into /src folder of your ros workspace.

`$ git clone -b master https://github.com/swri-robotics/mapviz.git`

Once the cloning is done, install all the dependencies using 

`$ rosdep install --from-paths src --ignore-src`

After the cloning you can build your workspace using,

`catkin build` or `catkin_make`

 whichever you have used previously.

 After this you should be able to launch mapviz using 

 `roslaunch mapviz mapviz.launch`

 If you see the mapviz screen, then your installation of mapviz is complete.

 ## Steps to configure your Mapviz launch file

 Navigate to the ***/src/mapviz/mapviz/launch*** folder of your workspace where you should be able to find a file called ***mapviz.launch***

The default launch file looks this this 
```
<launch>

  <node pkg="mapviz" type="mapviz" name="mapviz"></node>

  <node pkg="swri_transform_util" type="initialize_origin.py" name="initialize_origin" >
    <param name="local_xy_frame" value="/map"/>
    <param name="local_xy_origin" value="swri"/>
    <rosparam param="local_xy_origins">
      [{ name: swri,
         latitude: 29.45196669,
         longitude: -98.61370577,
         altitude: 233.719,
         heading: 0.0},
         
       { name: back_40,
         latitude: 29.447507,
         longitude: -98.629367,
         altitude: 200.0,
         heading: 0.0}]
    </rosparam>
  </node>

  <node pkg="tf" type="static_transform_publisher" name="swri_transform" args="0 0 0 0 0 0 /map /origin 100"  />



</launch>
```



 

 
