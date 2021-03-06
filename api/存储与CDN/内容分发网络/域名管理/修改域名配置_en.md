## 1. API Description
This API (UpdateCdnConfig) is used to modify the configuration information corresponding to the domain.

Domain for API request:<font style="color:red">cdn.api.qcloud.com</font>

1) Modify the configuration information of one domain at a time;
2) The caching rules, cache type, hotlink protection, origin HOST header, origin server and other configuration information can be modified for specific domains;
3) This API provides functions to [Modify origin server configuration](https://cloud.tencent.com/doc/api/231/1397) and [Modify caching rules](https://cloud.tencent.com/doc/api/231/3934).

[Call Demo](https://cloud.tencent.com/document/product/228/1734)

## 2. Input Parameters
The following request parameter list only provides API request parameters. Common request parameters need to be added when the API is called. See the [Common Request Parameters](https://cloud.tencent.com/doc/api/231/4473) page for details. The Action field for this API is UpdateCdnConfig.

| Parameter Name      | Required | Type          | Description                                       |
| --------- | ---- | ----------- | ---------------------------------------- |
| hostId    | Yes    | Int         | Domain ID to be set                              |
| projectId | No    | Int         | Project ID corresponding to the domain to be set                           |
| cache     | No    | JSON String | Cache configuration, as described below                             |
| cacheMode | No    | String      | Cache mode. There are two modes: "simple" means cache completely depends on the the Console; "custom" means cache depends on the cache expiration time set by the Console and the minimum value in max-age set by origin server.  |
| refer     | No    | JSON String | Hotlink protection configuration, as described below                             |
| fwdHost   | No    | String      | Origin host header; host parameter in the HTTP header sent from CDN node to origin.     |
| fullUrl   | No    | String      | Full-path URL cache. There are two modes: "on" means to enable; "off" means to disable.  |
| origin    | No    | String      | Configuration of the origin server                                     |

#### Details

##### Configuration of Hotlink Protection

Example:

```
[1,["qq.baidu.com", "*.baidu.com"]]
```

The first field specifies the type of refer:

+ 0:  Do not configure hotlink protection;
+ 1: Configure blacklist;
+ 2: Configure whitelist;

The second field is the specific namelist.

##### Configuration of Caching Rules

Example of cache configuration:

```
[[0,"all",1000],[1,".jpg;.js",2000],[2,"/www/html",3000],[3,"/index.html;/test/*.jpg",3000]]
```

The first parameter is cache type:

There are four types:

- 0: All types. This means all files are matched. This is the default cache configuration;
- 1: File type. This means matching based on file extensions;
- 2: Folder type. This means matching based on directories;
- 3: Full-path file. This means matching based on home page or matching a specified file.

The second parameter specifies matching rule:

- 0: "all" is entered in a fixed way;
- 1: Surfix, such as .jps; .js, separated with ";"
- 2: Directory, such as /www/html; /www/anc/, separated with ";"
- 3: Full-path file, such resource full-path as /index.html;/test/\*.jpg, separated with ";". "\*" can only be used for matching.

The third parameter specifies cache expiration time (in seconds).


## 3. Output Parameters

| Parameter Name     | Type     | Description                                       |
| -------- | ------ | ---------------------------------------- |
| code     | Int    | Common error code; 0: Succeeded; other values: Failed. For more information, refer to [Common Error Codes](https://cloud.tencent.com/doc/api/231/5078#1.-.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81) on Error Code page.  |
| message  | String | Module error message description depending on API.                           |
| codeDesc | String | English error message or error code at business side.                           |


## 4. Example

### 4.1 Example of Input

> hostId: 1234
> cache: [[0,"all",1000],[1,".jpg;.js",2000],[2,"/www/html",3000]]
> cacheMode: custom
> refer: [1,["qq.baidu.com", "*.baidu.com"]]



### 4.2 GET Request

All the parameters are required to be added after URL in GET request:

```
https://cdn.api.qcloud.com/v2/index.php?
Action=UpdateCdnConfig
&SecretId=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
&Timestamp=1462872270
&Nonce=541116052
&Signature=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
&hostId=207868
&cache=%5B%5B0%2C%22all%22%2C1000%5D%2C%5B1%2C%22.jpg%3B.js%22%2C2000%5D%2C%5B2%2C%22%2Fwww%2Fhtml%22%2C3000%5D%5D
&cacheMode=custom
&refer=%5B1%2C%5B%22qq.baidu.com%22%2C+%22%2A.baidu.com%22%5D%5D
```



### 4.3 POST Request

In POST request, the parameters will be filled in HTTP Request-body. The request address is:

```
https://cdn.api.qcloud.com/v2/index.php
```

Such formats of parameters as form-data, x-www-form-urlencoded are supported. The array of parameters is as follows:

```
array (
  'Action' => 'UpdateCdnConfig',
  'SecretId' => 'XXXXXXXXXXXXXXXXXXXXXXXXXXXX',
  'Timestamp' => 1462872294,
  'Nonce' => 479724541,
  'Signature' => 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
  'hostId' => '207868',
  'cache' => '[[0,"all",1000],[1,".jpg;.js",2000],[2,"/www/html",3000]]',
  'cacheMode' => 'custom',
  'refer' => '[1,["qq.baidu.com", "*.baidu.com"]]'
)
```


### 4.4 Example of Returned Result

#### Configuration Successful

```json
{
    "code": 0,
    "message": "",
    "codeDesc": "Success"
}
```

#### Configuration Failed

```json
{
    "code": 4000,
    "message": "(9130) refer error cdn invalid refer [refer should be json]",
    "codeDesc": 9130
}
```



