set(CMAKE_CXX_FLAGS "${GGS_CXX_FLAGS} ${CMAKE_CXX_FLAGS}")
set_property(DIRECTORY ${CMAKE_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES ${include_dirs})

configure_file(run.mac run.mac COPYONLY)
configure_file(run_pro_hs.mac run_pro_hs.mac COPYONLY)
configure_file(run_ele_hs.mac run_ele_hs.mac COPYONLY)
configure_file(run_pro_hs_100.mac run_pro_hs_100.mac COPYONLY)
configure_file(run_ele_hs_100.mac run_ele_hs_100.mac COPYONLY)
configure_file(run_pro_hs_1000.mac run_pro_hs_1000.mac COPYONLY)
configure_file(run_ele_hs_1000.mac run_ele_hs_1000.mac COPYONLY)
configure_file(geo.mac geo.mac COPYONLY)
configure_file(ToySegmentation.C ToySegmentation.C COPYONLY)
configure_file(ToyTiming.C ToyTiming.C COPYONLY)
configure_file(ElectronProton.cpp ElectronProton.cpp COPYONLY)

install(DIRECTORY . DESTINATION macros
       USE_SOURCE_PERMISSIONS
       PATTERN *~ EXCLUDE
       PATTERN "CMakeLists.txt" EXCLUDE
       )
