Affected Version:
openvswitch 3.3.0 33f45ded67a2d524ccf54cf4bb79a38d8140f14b

Vulnerability Description:
The defect is a post-release use defect located at line 1251 of the /ovs/ovsdb/transaction.c file, which could potentially be exploited maliciously, leading to program crashes and denial of service attacks.

openvswitch download address:
https://github.com/openvswitch/ovs.git

Detailed defect information:

1.Pass the pointer "txn" to the function "ovsdb_txn_precommit" at line 1249 of the file /ovs/ovsdb/transaction.c, as shown in the following figure:

![image](https://github.com/LuMingYinDetect/openvswitch_defects/blob/main/openvswitch_1.png)

2.When the conditional statement in line 1092 of the function ovsdb_txn_precommit evaluates to true, the program passes the variable "txn" to the function "ovsdb_txn_abort" at line 1093, as illustrated in the following diagram:

![image](https://github.com/LuMingYinDetect/openvswitch_defects/blob/main/openvswitch_2.png)

3.Pass the variable "txn" to the function "ovsdb_txn_free" at line 228 of the function ovsdb_txn_abort, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/openvswitch_defects/blob/main/openvswitch_3.png)

4.Call the free function to release the memory area corresponding to txn at line 152 of the function ovsdb_txn_free, as illustrated in the following diagram:

![image](https://github.com/LuMingYinDetect/openvswitch_defects/blob/main/openvswitch_4.png)

5.After the completion of the ovsdb_txn_precommit function, it returns a non-NULL pointer assigned to the variable "error." Therefore, the conditional statement at line 1250 of the program will evaluate to True, and the program will attempt to call the ovsdb_txn_abort function again at line 1251, aiming to release the pointer "txn" once more. This scenario constitutes a use-after-free vulnerability (also known as a double-free vulnerability), as depicted in the following diagram:

![image](https://github.com/LuMingYinDetect/openvswitch_defects/blob/main/openvswitch_5.png)
