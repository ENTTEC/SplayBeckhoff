# 70092 SPLAY BECKHOFF LIBRARY

This library contains the SPLAY Function Block for API integration with Beckhoff modules. For additional support, see the __[Official SPLAY API Documentation](https://github.com/ENTTEC/SplayApi)__. 

## INSTALLATION

1. Open the "References" folder in TwinCAT
2. Navigate to "Library repository"
3. Click "Install..." and select the SPLAY_API.compiled-library

> [ ! ] Make sure to restart TwinCAT after installing the library file on your local machine.

The SPLAY Function Block additionally requires two dependency libraries to be installed.
- *Tc3_JsonXml* provides the __[JsonDomParser](https://infosys.beckhoff.com/english.php?content=../content/1033/tcplclib_tc3_jsonxml/4219231115.html&id=)__ function block
- *Tc3_IotBase* provides the __[IotHttpClient](https://infosys.beckhoff.com/english.php?content=../content/1033/tf6760_tc3_iot_https_rest/7637099531.html&id=8676883144272531256)__ function block

## USAGE

Detailed examples using the SPLAY Function Block can be found in the MAIN POU of the Examples project. Only a general description will be provided here.

### **FB_SPLAY(fbJson, fbHttpClient)**

| Inputs         |                                                                                    |
|----------------|------------------------------------------------------------------------------------|
|'fbJson'        | The JsonDomParser function block                                                   |
|'fbHttpClient'  | The IotHttpClient function block                                                   |


| Outputs        |                                                                                    |
|----------------|------------------------------------------------------------------------------------|
|'jsonRX'        | An sJsonValue holding the request return (which can also be accessed from fbJson)  |

## EXAMPLE

```
PROGRAM MAIN
VAR
	fbJson				: FB_JsonDomParser;
	fbHttpClient		: FB_IotHttpClient:=(
								sHostName:='10.10.3.102',
								nHostPort:=80,
								bKeepAlive:=TRUE,
								tConnectionTimeout:=T#10S,
								stTLS:=(
									bNoServerCertCheck:=FALSE
								)
							);

	SPLAY				: FB_SPLAY:=(
							fbJson:=fbJson,
							fbHttpClient:=fbHttpClient
						);

	nState				: INT:=1;
	sReceived			: STRING(300);
END_VAR
```
```
CASE nState OF
	1:
        // Example 'GET_INFO' command usage

		SPLAY.GET_INFO();
		fbJson.CopyDocument(sReceived, SIZEOF(sReceived));
		ADSLOGSTR(												
			msgCtrlMask:=ADSLOG_MSGTYPE_STRING,					
			msgFmtStr:='%s',									
			strArg:=sReceived							
		);
		
		nState:=2;
		
END_CASE
```

## REMARKS

- Latest version v1.0.0.1.

- Input buffers are currently limited to 3kB of memory.

- Currently, support has not been implemented for the following functions:
    
    - GET_PLAYLIST_INTENSITY
    
    - SET_TRACK_INTENSITY
    
    - GET_TRACK_INTENSITY
    
    - UPDATE_STORAGE_PATH
    
    - UPDATE_SCHEDULE
    
    - DELETE_SCHEDULE
    
    - ENABLE_SCHEDULE
    
    - SET TIME
    
    - REFRESH_SETTING
