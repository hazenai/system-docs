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

* `stream_type` - 
* `stream_address` -
* `sitename_camname` - 
* `timezone_of_video_file` - 
* `timeout_to_read_frame_sec` - 
* `delay_to_getframe_ms` -
* `jpeg_encoded`
* `pre_processor_type` -
* `img_resize_width` - 
* `img_resize_height` -
* `change_color_space_to_rgb` -
* `increase_larger_side_by_factor` -
* `increase_smaller_side_by_factor` -
* `std_for_preprocess_img` -
* `mean_for_preprocess_img` -
* `traffic_light_config` - 
* `do_resize_in_preprocessor` - 
* `infer_type` - 
* `output_node_ids` - 
* `engine_file_path` -
* `batch_size` - 
* `trt_gpu_workspace_size` - 
* `enable_trt_preprocessor` -
* `trt_preprocessor_input_dims` - 
* `image_dims_infer_engine` -
* `post_processor_type` -
* `all_states` -
* `class_map` -
* `state_machine_type` -
* `yellow_ticks_statemachine` -
* `yellow_tol_statemachine` -
* `green_th_state_statemachine` -
* `invalid_th_statemachine` -
* `yellow_floor_th_statemachine` -
* `yellow_ceil_th_statemachine` -
* `yellow_hist_buffer_statemachine` -
* `thres_hold_statemachine` -
* `group_count` -
* `lights_per_group` -
* `classification_threshold` -
* `sensor_fusion_type` -
* `debug_log` -
* `redis_host` -
* `redis_port` -
* `connection_timeout_secs` -
* `topic` -
* `output_type` -
* `group1_queue_id` -
* `group2_queue_id` -
* `config_trigger_queue` -

## Object Tracker

## Violation Service

## Aggregator

## Publisher

