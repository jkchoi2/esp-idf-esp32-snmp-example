file(GLOB_RECURSE SNMP_SOURCES "${IDF_PATH}/components/lwip/lwip/src/apps/snmp/*.c" )

idf_component_register(	SRCS "main.c" "uto_eth_init.c"  "my_mib.c" "simple_snmp_agent.c" ${SNMP_SOURCES}
                       	PRIV_REQUIRES nvs_flash spi_flash spiffs esp_event esp_eth esp_netif lwip driver
                       	INCLUDE_DIRS "."
						${IDF_PATH}/components/lwip/lwip/src/apps/snmp
        				${IDF_PATH}/components/lwip/lwip/src/include
					)

#LWIP Options override
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DMIB2_STATS=1 -DLWIP_SNMP=1 -DSNMP_USE_RAW=0 -DSNMP_USE_NETCONN=1 -DSNMP_LWIP_MIB2=1")

set_source_files_properties(
  ${IDF_PATH}/components/lwip/lwip/src/apps/snmp/snmp_traps.c
  PROPERTIES COMPILE_FLAGS "-Wno-dangling-pointer")

spiffs_create_partition_image(storage ../data FLASH_IN_PROJECT)
