#tetrapak 

--- 



**ESEMPIO LOW SPEED DATALOG**

	riga in cui sono definite caratteristiche custom   -> 131 main.c
																							  -> 497 main.c
																							  -> 307 sensor_service.c

	riga di aci_gatt_read_char_value(uint16_t Connection_Handle, uint16_t Attr_Handle);
																								-> 702 bluenrg1_gatt_aci.h

	riga in cui viene creato il servizio con la funzione Add_ConfigW2ST_Service()
																								-> 508 main.c
											funzione definita alla riga   -> 124 sensor_service.c

	Attribute_Modified_CB() callback chiamata quando è modificato un attributo nel GATT database.                                              -> 685 sensor_service.c
	                                                                                             -> 911 sensor_service.c


**ESEMPIO HIGH SPEED DATALOG**

	aci_gatt_attribute_modified_event   -> 1171 ble_config_service.c

	Lettura diretta da registri -> ism330dhcx_app.c   funzione ISM330DHCX_Read_Data()
																				funzione ISM330DHCX_Read_Data_From_FIFO()