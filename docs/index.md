# Welcome to Hazen.ai System Documentation

## Image Cache

* `sensor_type` - 'rtsp' is used for rtsp stream, file is used for video file source, roseek for roseek camera
* `cam_id` - file path or stream addresss
* `encoder` -  video reader encoder, supported values are "gstreamer", "ffmpeg" & "v4l2" DEFAULT is "ffmpeg"
* `hardware_wait_sec` - timeout value in seconds if the frame reading process from source is interrupted
* `delay_for_rtsp_ms` - delay introduced in frames in case of video file, should be 0 for rtsp
* `source_frame_rate` - should only be set for ROSEEK, doesn't do anything in case of video file and rtsp stream
* `jpeg_encoding` - set to true if jpeg encoding is used in the pipeline
* `size` - capacity of image cache in frames
* `debug_log` - sets the debug log level to true or false, default log leve is INFO
* `video_frame_rate` -  sets the debug log level to true or false, default log leve is INFO
* `color` - color format of frames, can be true or false
* `grpc_server_host` - default 0.0.0.0
* `grpc_server_port` - default 5000
* `threshold_ms` -  default 1000000
* `server_wait_sec` - default 15
* `publish_on_redis` - this parameter is true if a client requires a latest frame, should be false for ALPR cameras
* `redis_server_host` - default 0.0.0.0
* `redis_server_port` - default 7000
* `redis_topic` - default "source_frames"
* `redis_timeout` - default 2

## Object Detector

* `stream_type` - 'gRPC' used to get images from image-cache and other options are rtsp, udp and file
* `stream_address` - file path or stream addresss
* `sitename_camname` - name of site
* `timezone_of_video_file` - in case of file, this is to specify the timezone of file.
* `timeout_to_read_frame_sec` - timeout value in seconds if the frame reading process from source is interrupted
* `delay_to_getframe_ms` - value in milliseconds to slow down frame reading process.
* `jpeg_encoded` - set to true if jpeg encoding is used in image-cache.
* `pre_processor_type` - specify the preprocessor you want to use depend upon the problem. possible values are `traffic`, `seatbelt_mobilephone` and `trajectory`.
* `img_resize_width` - set width to which you want to resize image in 'traffic' preprocessor.
* `img_resize_height` - set height to which you want to resize image in 'traffic' preprocessor.
* `change_color_space_to_rgb` - set true if want to change BGR image to RGB.
* `increase_larger_side_by_factor` - default 0.25, only for TLPD, to increase BBox of traffic light.
* `increase_smaller_side_by_factor` - default 0.25, only for TLPD, to increase BBox of traffic light.
* `std_for_preprocess_img` - default [0.229, 0.224, 0.225], set values to perform std of image. only for TLPD
* `mean_for_preprocess_img` - default [0.229, 0.224, 0.225], set values to perform mean of image. only for TLPD
* `traffic_light_config` - path to trafficlights.json file.
* `do_resize_in_preprocessor` - set true if you want to do resizing in preprocessor.
* `infer_type` - default 'tensorrt' for gpu based inference and openvino20 for cpu based infer.
* `output_node_ids` - only for openvino20 infer, depends upon engine.
* `engine_file_path` - path to engine file without extensions.
* `batch_size` - batch size passed to inference.
* `trt_gpu_workspace_size` - default '2.4' set trt gpu workspace size in Gibs
* `enable_trt_preprocessor` - set true if want to perform gpu based preprocessing.
* `trt_preprocessor_input_dims` - default [1080,1920,3] image dims, passed to gpu preprocessor in NHWC (Height, Width, channels) format.
* `image_dims_infer_engine` - default [3,1080,1980] image dims on which main infer engine will work in NCHW (channels, width, Height) format.
* `post_processor_type` - postprocessor you want. possible values are `tlstatus`, `seatbelt_mobilephone` and `trajectory`
* `all_states` - ["red", "green", "yellow", "black"], only for tlstatus to represent all states of trafficlight.
* `class_map` - "0,1,2,3|black,green,red,yellow" To represent infer result with respective classes. Before '|' is output of infer and after are the respective classes.
* `state_machine_type` - 'dynamic', only for tlstatus postprocessor. possible values dynamic and constant
* `yellow_ticks_statemachine` - default 18, only for tlstatus postprocessor
* `yellow_tol_statemachine` - default 5
* `green_th_state_statemachine` - default 2
* `invalid_th_statemachine` - default 10
* `yellow_floor_th_statemachine` - default 2
* `yellow_ceil_th_statemachine` - default 5
* `yellow_hist_buffer_statemachine` - default 10
* `thres_hold_statemachine` - default 1
* `group_count` - default 2, total groups of trafficlight.
* `lights_per_group` - default [5, 0], total lights per group.
* `classification_threshold` - default 0.45, classification threshold for tlstatus postprocessor
* `sensor_fusion_type` - default 'majority', possible values are mean and majority
* `debug_log` - set true if want to see debug logs
* `redis_host` - redis host on which redis-server is running
* `redis_port` - port redis-server is running
* `connection_timeout_secs` - timeout value in seconds to connect to redis.
* `topic` - topic on which OD will publish packages.
* `output_type` - Specify type on which you want to get output of OD. Possible values are redis, mqtt and system_v_queue( only working for one specific case)
* `group1_queue_id` - queue id of system-v-queue on which want to get phase change status of group1(only for one specific case)
* `group2_queue_id` -  queue id of system-v-queue on which want to get phase change status of group2(only for one specific case)
* `config_trigger_queue` - queue id on which OD will get notification about config change. (only for specific case)

## Object Tracker

* `tracker_type` - default 'hazenv2'
* `use_objectness_score` - For SBMP deployment use `False`, True otherwise
* `tracked_classes` - Tracker will track these classes for label smoothing. `vehicle_type` for trajectory and `seatbelt|mobile_phone` for SBMP
* `log_level` - default INFO, other value can be `DEBUG`
* `redis_host` - default 0.0.0.0, IP/domain of redis server
* `redis_port` - default 6379, port at which redis server is accessible
* `listen_topic` - tracker will subscribe to this topic for listening detections
* `publish_topic` - tracker will publish tracks to this topic
* `sigma_l (float)` - low score detection threshold
* `sigma_iou (float)` - default -0.05
* `sigma_len (int)` - minimum track length in frames
* `sigma_p (int)` - maximum frames a track remains pending before termination
* `sigma_h (float)` - At least one detection in a track should have score greater than this
* `output_pending_tracks` - default False
* `motion_model` - default 'cppkalman'
* `association_model` - default 'hungarian'
* `cost_model` - default 'iou'

## Violation Service

## Aggregator

* `camera_cache_mapping` -
* `request_image_encoding` - Get image of specific encoding. Typical values are `jpg`, `png`. 
* `request_image_offset_allowed_ms` -
* `queue_address` -
* `queue_port` -
* `queue_qos` - 
* `queue_keep_alive` - 
* `queue_event_topic` -
* `queue_violation_service_topic` - 
* `queue_publisher_service_topic` -
* `event_storage_expiry` -
* `container_storage` -
* `log_level` -
* `generate_dummy_violation` -
* `map_to_wrongturn` -
* `map_to_runningredlight` -
* `map_to_trespass_zebra` -
* `map_to_cellphone` -
* `map_to_seatbelt` -
* `publish_violations` -
* `redis_host` -
* `redis_port` -

## Publisher

* `package_receive_topic` -
* `edge_mqtt_address` -
* `edge_mqtt_port` -
* `package_receive_queue_qos` -
* `package_receive_queue_keep_alive` -
* `container_storage` -
* `cloud_mqtt_address` -
* `cloud_mqtt_port` -
* `cloud_publish_topic` -
* `cloud_queue_qos` -
* `cloud_queue_keep_alive` -
* `log_level` -
* `remote_publisher_enable_ssl` -
* `ca_certs` -
* `client_cert` -
* `client_key` -
* `redis_host` -
* `redis_port`
