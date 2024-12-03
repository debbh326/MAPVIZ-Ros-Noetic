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

 ## Steps to configure your mapviz launch file

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
When you launch mapviz, the launch file also launches the script ***initialize_origin.py*** from the ***swri_transform_util*** node. Now there are a couple of parameters that one must understand to use it properly.

Here is the launch file I am using:

```
<launch>

  <node pkg="mapviz" type="mapviz" name="mapviz"></node>

  <node pkg="swri_transform_util" type="initialize_origin.py" name="initialize_origin" >
    <param name="local_xy_frame" value="/map"/>
    <param name="local_xy_origin" value="auto"/>
    <param name="local_xy_navsatfix_topic" value="/ublox/fix"/>
    <remap from="fix" to="/ublox/fix"/>
    <rosparam param="local_xy_origins">
      [{ name: hkust,
         latitude: 22.335824,
         longitude: 114.263865,
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
`<param name="local_xy_origin" value="auto"/>` This parameter makes the initialize_origin.py script to take the first valid reading of the GPS and set it as the origin for mapviz to use.

`<remap from="fix" to="/ublox/fix"/>` This parameter changes the default topic of NavSatFix in the initialize_origin.py to subscribe to the topic "/ublox/fix" that is my GPS's NavSatFix topic.

If you were to use GPScommon message type instead use `<remap from="gps" to="your gps fix topic"/>`

If we dont set `<param name="local_xy_origin" value="auto"/>` and instead `<param name="local_xy_origin" value="swri"/>` or `<param name="local_xy_origin" value="hkust"/>` in my case, we will set the origin as the location defined in the ros-parameters which is also useful in certain cases.

## Setup in mapviz

Make sure to add the elements in order becuase the map will build the lower ones on top of the ones above, so add tile_map first to see the maps, then add the NavSat. 

In the tile_map, the default stamen maps is no longer in service, so I would recommend to use Bing Maps(with your API) or make an account in Stadia Maps and use the tiles API to get the tile maps view.





 

 
