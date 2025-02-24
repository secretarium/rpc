cmake_minimum_required(VERSION 3.12)

message("common_enclave")

# ##############################################################################
# common_enclave

if(${BUILD_ENCLAVE})
    add_library(common_enclave STATIC
    

    src/host_service_proxy.cpp
    )
   
   target_compile_definitions(common_enclave PRIVATE ${ENCLAVE_DEFINES})
   target_include_directories(common_enclave 
   PUBLIC 
     "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
     "$<INSTALL_INTERFACE:include/marshalled_tests>"
     PRIVATE 
       ${ENCLAVE_LIBCXX_INCLUDES})
     
   target_compile_options(common_enclave PRIVATE ${ENCLAVE_COMPILE_OPTIONS})
   target_link_options(common_enclave PRIVATE ${ENCLAVE_LINK_OPTIONS})
  
  
   target_link_libraries(common_enclave 
   PUBLIC
     marshaller_enclave
     marshal_test_edl_enclave

     yas_common    
    )
  
   set_property(TARGET common_enclave PROPERTY COMPILE_PDB_NAME common_enclave)
   add_dependencies(common_enclave example_interface)
  

endif()
if(${BUILD_HOST})

 add_library(common_host STATIC

  src/enclave_service_proxy.cpp
  )
 
 target_compile_definitions(common_host PRIVATE ${HOST_DEFINES})
 target_include_directories(common_host 
 PUBLIC 
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/marshalled_tests>"
   PRIVATE 
     ${HOST_INCLUDES})
   
 target_compile_options(common_host PRIVATE ${HOST_COMPILE_OPTIONS})
 target_link_options(common_host PRIVATE ${HOST_LINK_OPTIONS})


 target_link_libraries(common_host 
 PUBLIC
   marshaller_host 
   marshal_test_edl_host

   yas_common    
 )

 set_property(TARGET common_host PROPERTY COMPILE_PDB_NAME common_host)
 add_dependencies(common_host example_interface)

 endif()