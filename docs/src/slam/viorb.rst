.. _slam_viorb:

`VIORB <https://github.com/jingpang/LearnVIORB>`_ 如何整合
=============================================================


在 MYNT® EYE 上运行 VIORB ，请依照这些步骤：
------------------------------------------------

1. 下载 `MYNT-EYE-S-SDK <https://github.com/slightech/MYNT-EYE-S-SDK.git>`_ ， 安装 mynt_eye_ros_wrapper。
2. 按照一般步骤安装 VIORB 。
3. 更新相机参数到 ``<VIO>/config/mynteye_s.yaml``。
4. 运行 mynt_eye_ros_wrapper 和 VIORB 。

安装 MYNT-EYE-VIORB-Sample.
---------------------------

.. code-block:: bash

  git clone -b mynteye https://github.com/slightech/MYNT-EYE-VIORB-Sample.git
  cd MYNT-EYE-VIORB-Sample

添加 ``Examples/ROS/ORB_VIO`` 路径到环境变量 ``ROS_PACKAGE_PATH`` 。打开 ``.bashrc`` 文件，在最后添加下面命令行。 ``PATH`` 为当前 ``MYNT-EYE-VIORB-Sample.`` 存放路径:

.. code-block:: bash

  export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:PATH/Examples/ROS/ORB_VIO

执行:

.. code-block:: bash

  cd MYNT-EYE-VIORB-Sample
  ./build.sh

获取相机校准参数
-----------------

使用 MYNT® EYE 的左目摄像头和 IMU 。通过 `MYNT-EYE-S-SDK <https://github.com/slightech/MYNT-EYE-S-SDK.git>`_ API 的 ``GetIntrinsics()`` 函数和 ``GetExtrinsics()`` 函数，可以获得当前工作设备的图像校准参数：

.. code-block:: bash

  cd MYNT-EYE-S-SDK
  ./samples/_output/bin/tutorials/get_img_params

这时，可以获得针孔模型下的 ``distortion_parameters`` 和 ``projection_parameters`` 参数，然后在 ``<MYNT-EYE-VIORB-Sample>/config/mynteye_s.yaml`` 中更新。

.. tip::

  获取相机校准参数时可以看到相机模型，
  -如果相机为等距模型不能直接写入参数，需要自己标定针孔模型或者按照 :ref:`write_img_params` 写入SDK中的针孔模型参数来使用。

  
运行 VIORB 和 mynt_eye_ros_wrapper
--------------------------------------

1.运行mynteye节点

.. code-block:: bash

  roslaunch mynt_eye_ros_wrapper mynteye.launch

2.打开另一个命令行运行viorb

.. code-block:: bash

  roslaunch ORB_VIO testmynteye_s.launch

最后，``pyplotscripts`` 下的脚本会将结果可视化。

