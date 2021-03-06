cmake_minimum_required(VERSION 2.8.3)

find_package(catkin REQUIRED COMPONENTS roscpp rostest nav_msgs tf)


if(MCL_3DL_EXTRA_TESTS)

	catkin_download_test_data(
			${PROJECT_NAME}_short_test.bag
			https://openspur.org/~atsushi.w/dataset/mcl_3dl/short_test.bag
			MD5 664ecb2824d81247ce4b26c599d99230
			FILENAME test/short_test.bag)

	catkin_download_test_data(
			${PROJECT_NAME}_short_test_ref.topic
			https://openspur.org/~atsushi.w/dataset/mcl_3dl/short_test_ref.topic
			MD5 1087915e65da06b55e0cc5e4377b485d
			FILENAME test/short_test_ref.topic)

	configure_file(tests/localization_rostest.test.in
			${PROJECT_BINARY_DIR}/test/tests/localization_rostest.test)
	configure_file(tests/tf_rostest.test.in
			${PROJECT_BINARY_DIR}/test/tests/tf_rostest.test)

	add_rostest_gtest(compare_pose ${PROJECT_BINARY_DIR}/test/tests/localization_rostest.test
			src/compare_pose.cpp)
	target_link_libraries(compare_pose ${catkin_LIBRARIES})
	add_dependencies(compare_pose 
				${PROJECT_NAME}_short_test.bag
				${PROJECT_NAME}_short_test_ref.topic 
			)
	add_rostest_gtest(compare_tf ${PROJECT_BINARY_DIR}/test/tests/tf_rostest.test
			src/compare_tf.cpp)
	target_link_libraries(compare_tf ${catkin_LIBRARIES})
	add_dependencies(compare_tf 
				${PROJECT_NAME}_short_test.bag
				${PROJECT_NAME}_short_test_ref.topic 
			)

else(MCL_3DL_EXTRA_TESTS)

	message(WARNING "To perform extra heavy tests for mcl_4dl, use -DMCL_3DL_EXTRA_TESTS:=ON")

endif(MCL_3DL_EXTRA_TESTS)

