## SDK事件回调

### TIMRecvNewMsgCallback

新消息回调

**原型：**

```c
typedef void (*TIMRecvNewMsgCallback)(const char* json_msg_array, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| json_msg_array | const char* | 新消息数组 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**注释：**

>此回调可以获取新接收的消息数组。注意 消息内的元素也是一个数组。每个元素的定义由elem_type字段决定


**示例： 1. 消息数组解析示例**

```c
 Json::Value json_value_msgs; // 解析消息
 Json::Reader reader;
 if (!reader.parse(json_msg_array, json_value_msgs)) {
     printf("reader parse failure!%s", reader.getFormattedErrorMessages().c_str());
     return;
 }
 for (Json::ArrayIndex i = 0; i < json_value_msgs.size(); i++) {  // 遍历Message
     Json::Value& json_value_msg = json_value_msgs[i];
     Json::Value& elems = json_value_msg[kTIMMsgElemArray];
     for (Json::ArrayIndex m = 0; m < elems.size(); m++) {   // 遍历Elem
         Json::Value& elem = elems[i];

         uint32_t elem_type = elem[kTIMElemType].asUInt();
         if (elem_type == TIMElemType::kTIMElem_Text) {  // 文本
             
         } else if (elem_type == TIMElemType::kTIMElem_Sound) {  // 声音
             
         } else if (elem_type == TIMElemType::kTIMElem_File) {  // 文件
             
         } else if (elem_type == TIMElemType::kTIMElem_Image) { // 图片
             
         } else if (elem_type == TIMElemType::kTIMElem_Custom) { // 自定义元素
             
         } else if (elem_type == TIMElemType::kTIMElem_GroupTips) { // 群组系统消息
             
         } else if (elem_type == TIMElemType::kTIMElem_Face) { // 表情
             
         } else if (elem_type == TIMElemType::kTIMElem_Location) { // 位置
             
         } else if (elem_type == TIMElemType::kTIMElem_GroupReport) { // 群组系统通知
             
         } else if (elem_type == TIMElemType::kTIMElem_Video) { // 视频
             
         }
     }
 }
 
```

**示例： 2. 返回一个文本消息的Json示例。Json Key参考 [Message](Url_for_TIMCloudDef#Message) 、 [TextElem](Url_for_TIMCloudDef#TextElem) **

```c
 [
    {
       "message_client_time" : 1551080111,
       "message_conv_id" : "user2",
       "message_conv_type" : 1,
       "message_elem_array" : [
          {
             "elem_type" : 0,
             "text_elem_content" : "123213213"
          }
       ],
       "message_is_from_self" : true,
       "message_is_read" : true,
       "message_rand" : 2130485001,
       "message_sender" : "user1",
       "message_seq" : 1,
       "message_server_time" : 1551080111,
       "message_status" : 2
    }
 ]
```

**示例： 3. 返回一个群通知消息的Json示例。Json Key参考 [Message](Url_for_TIMCloudDef#Message) 、 [GroupReportElem](Url_for_TIMCloudDef#GroupReportElem) **

```c
 [
    {
       "message_client_time" : 1551344977,
       "message_conv_id" : "",
       "message_conv_type" : 3,
       "message_elem_array" : [
          {
             "elem_type" : 9,
             "group_report_elem_group_id" : "first group id",
             "group_report_elem_group_name" : "first group name",
             "group_report_elem_msg" : "",
             "group_report_elem_op_group_memberinfo" : {
                "group_member_info_custom_info" : {},
                "group_member_info_identifier" : "user1",
                "group_member_info_join_time" : 0,
                "group_member_info_member_role" : 0,
                "group_member_info_msg_flag" : 0,
                "group_member_info_msg_seq" : 0,
                "group_member_info_name_card" : "",
                "group_member_info_shutup_time" : 0
             },
             "group_report_elem_op_user" : "",
             "group_report_elem_platform" : "Windows",
             "group_report_elem_report_type" : 6,
             "group_report_elem_user_data" : ""
          }
       ],
       "message_is_from_self" : false,
       "message_is_read" : true,
       "message_rand" : 2207687390,
       "message_sender" : "@TIM#SYSTEM",
       "message_seq" : 1,
       "message_server_time" : 1551344977,
       "message_status" : 2
    }
 ]
 
```

**示例： 4. 返回一个群提示消息的Json示例。Json Key参考 [Message](Url_for_TIMCloudDef#Message) 、 [GroupTipsElem](Url_for_TIMCloudDef#GroupTipsElem) **

```c
 [
    {
       "message_client_time" : 1551412814,
       "message_conv_id" : "first group id",
       "message_conv_type" : 2,
       "message_elem_array" : [
          {
             "elem_type" : 6,
             "group_tips_elem_changed_group_memberinfo_array" : [],
             "group_tips_elem_group_change_info_array" : [
                {
                   "group_tips_group_change_info_flag" : 10,
                   "group_tips_group_change_info_value" : "first group name to other name"
                }
             ],
             "group_tips_elem_group_id" : "first group id",
             "group_tips_elem_group_name" : "first group name to other name",
             "group_tips_elem_member_change_info_array" : [],
             "group_tips_elem_member_num" : 0,
             "group_tips_elem_op_group_memberinfo" : {
                "group_member_info_custom_info" : {},
                "group_member_info_identifier" : "user1",
                "group_member_info_join_time" : 0,
                "group_member_info_member_role" : 0,
                "group_member_info_msg_flag" : 0,
                "group_member_info_msg_seq" : 0,
                "group_member_info_name_card" : "",
                "group_member_info_shutup_time" : 0
             },
             "group_tips_elem_op_user" : "user1",
             "group_tips_elem_platform" : "Windows",
             "group_tips_elem_time" : 0,
             "group_tips_elem_tip_type" : 6,
             "group_tips_elem_user_array" : []
          }
       ],
       "message_is_from_self" : false,
       "message_is_read" : true,
       "message_rand" : 1,
       "message_sender" : "@TIM#SYSTEM",
       "message_seq" : 1,
       "message_server_time" : 1551412814,
       "message_status" : 2
    },
 ]

```

### TIMMsgReadedReceiptCallback

新消息回调

**原型：**

```c
typedef void (*TIMMsgReadedReceiptCallback)(const char* json_msg_readed_receipt_array, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| json_msg_readed_receipt_array | const char* | 消息已读回执数组 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**示例：**

```c
 void MsgReadedReceiptCallback(const char* json_msg_readed_receipt_array, const void* user_data) {
     Json::Value json_value_receipts;
     Json::Reader reader;
     if (!reader.parse(json_msg_readed_receipt_array, json_value_receipts)) {
         // Json 解析失败
         return;
     }
     
     for (Json::ArrayIndex i = 0; i < json_value_receipts.size(); i++) {
         Json::Value& json_value_receipt = json_value_receipts[i];
     
         std::string convid = json_value_receipt[kTIMMsgReceiptConvId].asString();
         uint32_t conv_type = json_value_receipt[kTIMMsgReceiptConvType].asUInt();
         uint64_t timestamp = json_value_receipt[kTIMMsgReceiptTimeStamp].asUInt64();
     
         // 消息已读逻辑
     }
 }

```

### TIMMsgRevokeCallback

新消息回调

**原型：**

```c
typedef void (*TIMMsgRevokeCallback)(const char* json_msg_locator_array, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| json_msg_locator_array | const char* | 消息定位符数组 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**示例：**

```c
 void MsgRevokeCallback(const char* json_msg_locator_array, const void* user_data) {
     Json::Value json_value_locators;
     Json::Reader reader;
     if (!reader.parse(json_msg_locator_array, json_value_locators)) {
         // Json 解析失败
         return;
     }
     for (Json::ArrayIndex i = 0; i < json_value_locators.size(); i++) {
         Json::Value& json_value_locator = json_value_locators[i];
     
         std::string convid = json_value_locator[kTIMMsgLocatorConvId].asString();
         uint32_t conv_type = json_value_locator[kTIMMsgLocatorConvType].asUInt();
         bool isrevoke      = json_value_locator[kTIMMsgLocatorIsRevoked].asBool();
         uint64_t time      = json_value_locator[kTIMMsgLocatorTime].asUInt64();
         uint64_t seq       = json_value_locator[kTIMMsgLocatorSeq].asUInt64();
         uint64_t rand      = json_value_locator[kTIMMsgLocatorRand].asUInt64();
         bool isself        = json_value_locator[kTIMMsgLocatorIsSelf].asBool();
     
         // 消息撤回逻辑
     }
 }
 

```

### TIMMsgElemUploadProgressCallback

新消息回调

**原型：**

```c
typedef void (*TIMMsgElemUploadProgressCallback)(const char* json_msg, uint32_t index, uint32_t cur_size, uint32_t total_size, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| json_msg | const char* | 新消息 |
| index | uint32_t | 上传Elem元素在json_msg消息的下标 |
| cur_size | uint32_t | 上传当前大小 |
| total_size | uint32_t | 上传总大小 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**示例：**

```c
 void MsgElemUploadProgressCallback(const char* json_msg, uint32_t index, uint32_t cur_size, uint32_t total_size, const void* user_data) {
     Json::Value json_value_msg;
     Json::Reader reader;
     if (!reader.parse(json_msg, json_value_msg)) {
         // Json 解析失败
         return;
     }
     Json::Value& elems = json_value_msg[kTIMMsgElemArray];
     if (index >= elems.size()) {
         // index 超过消息元素个数范围
         return;
     }
     uint32_t elem_type = elems[index][kTIMElemType].asUInt();
     if (kTIMElem_File ==  elem_type) {
 
     }
     else if (kTIMElem_Sound == elem_type) {
 
     }
     else if (kTIMElem_Video == elem_type) {

     }
     else if (kTIMElem_Image == elem_type) {

     }
     else {
         // 其他类型元素不符合上传要求
     }
 }

```

### TIMGroupTipsEventCallback

群事件回调

**原型：**

```c
typedef void (*TIMGroupTipsEventCallback)(const char* json_group_tip_array, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| json_group_tip_array | const char* | 群提示列表 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

### TIMConvEventCallback

会话事件回调

**原型：**

```c
typedef void (*TIMConvEventCallback)(TIMConvEvent conv_event, const char* json_conv_array, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| conv_event | TIMConvEvent | 会话事件类型。 [TIMConvEvent](Url_for_TIMCloudDef#TIMConvEvent)  |
| json_conv_array | const char* | 会话信息列表 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**示例： 会话事件回调数据解析**

```c
 void ConvEventCallback(TIMConvEvent conv_event, const char* json_conv_array, const void* user_data) {
     Json::Reader reader;
     Json::Value json_value;
     if (!reader.parse(json_conv_array, json_value)) {
         // Json 解析失败
         return;
     }
     for (Json::ArrayIndex i = 0; i < json_value.size(); i++) { // 遍历会话类别
         Json::Value& convinfo = json_value[i];
         // 区分会话事件类型
         if (conv_event == kTIMConvEvent_Add) {

         }
         else if (conv_event == kTIMConvEvent_Del) {

         }
         else if (conv_event == kTIMConvEvent_Update) {

         }
     }
 }

```

### TIMNetworkStatusListenerCallback

会话事件回调

**原型：**

```c
typedef void (*TIMNetworkStatusListenerCallback)(TIMNetworkStatus status, int32_t code, const char* desc, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| status | TIMNetworkStatus | 日志级别。 [TIMNetworkStatus](Url_for_TIMCloudDef#TIMNetworkStatus)  |
| code | int32_t | ERR_SUCC表示成功，其他值表示失败。详情见  [TIMErrCode](Url_for_TIMCloudDef#TIMErrCode)  |
| desc | const char* | 错误描述字符串 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**示例： 感知网络状态的回调处理**

```c
 void NetworkStatusListenerCallback(TIMNetworkStatus status, int32_t code, const char* desc, const void* user_data) {
     switch(status) {
     case kTIMConnected: {
         printf("OnConnected ! user_data:0x%08x", user_data);
         break;
     }
     case kTIMDisconnected:{
         printf("OnDisconnected ! user_data:0x%08x", user_data);
         break;
     }
     case kTIMConnecting:{
         printf("OnConnecting ! user_data:0x%08x", user_data);
         break;
     }
     case kTIMConnectFailed:{
         printf("ConnectFailed code:%u desc:%s ! user_data:0x%08x", code, desc, user_data);
         break;
     }
     }
 }

```

### TIMKickedOfflineCallback

被踢下线回调

**原型：**

```c
typedef void (*TIMKickedOfflineCallback)(const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

### TIMUserSigExpiredCallback

用户票据过期回调

**原型：**

```c
typedef void (*TIMUserSigExpiredCallback)(const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

### TIMLogCallback

日志回调

**原型：**

```c
typedef void (*TIMLogCallback)(TIMLogLevel level, const char* log, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| level | TIMLogLevel | 日志级别。 [TIMLogLevel](Url_for_TIMCloudDef#TIMLogLevel)  |
| log | const char* | 日子字符串 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

### TIMMsgUpdateCallback

消息更新回调

**原型：**

```c
typedef void (*TIMMsgUpdateCallback)(const char* json_msg_array, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| json_msg_array | const char* | 更新的消息数组 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**注释：**

>参考  [TIMRecvNewMsgCallback](#TIMRecvNewMsgCallback) 




## SDK接口回调

### TIMCommCallback

接口回调定义

**原型：**

```c
typedef void (*TIMCommCallback)(int32_t code, const char* desc, const char* json_params, const void* user_data);
```

**参数**

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| code | int32_t | ERR_SUCC表示成功，其他值表示失败。详情见  [TIMErrCode](Url_for_TIMCloudDef#TIMErrCode)  |
| desc | const char* | 错误描述字符串 |
| json_params | const char* | Json字符串，不同的接口，Json字符串不一样 |
| user_data | const void* | SDK负责透传的用户自定义数据，未做任何处理 |

**注释：**

>所有回调均需判断code是否等于ERR_SUC，若不等于说明接口调用失败了，具体原因可以看code的值以及desc描述。详细请看 [错误码](https://cloud.tencent.com/document/product/269/1671#错误码) 


**示例： 接口 [TIMSetConfig](Url_for_TIMCloud#TIMSetConfig) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [SetConfig](Url_for_TIMCloudDef#SetConfig) 。**

```c
 {
    "set_config_callback_log_level" : 2,
    "set_config_is_log_output_console" : true,
    "set_config_log_level" : 2,
    "set_config_proxy_info" : {
       "proxy_info_ip" : "",
       "proxy_info_port" : 0
    },
    "set_config_user_config" : {
       "user_config_group_getinfo_option" : {
          "get_info_option_custom_array" : [],
          "get_info_option_info_flag" : 0xffffffff,
          "get_info_option_role_flag" : 0
       },
       "user_config_group_member_getinfo_option" : {
          "get_info_option_custom_array" : [],
          "get_info_option_info_flag" : 0xffffffff,
          "get_info_option_role_flag" : 0
       },
       "user_config_is_ingore_grouptips_unread" : false,
       "user_config_is_read_receipt" : false,
       "user_config_is_sync_report" : false
    }
 }
```

**示例： 接口 [TIMConvCreate](Url_for_TIMCloud#TIMConvCreate) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [ConvInfo](Url_for_TIMCloudDef#ConvInfo) 。**

```c
 {
    "conv_active_time" : 1551269275,
    "conv_id" : "user2",
    "conv_is_has_draft" : false,
    "conv_is_has_lastmsg" : true,
    "conv_last_msg" : {
       "message_client_time" : 1551101578,
       "message_conv_id" : "user2",
       "message_conv_type" : 1,
       "message_elem_array" : [
          {
             "elem_type" : 0,
             "text_elem_content" : "12"
          }
       ],
       "message_is_from_self" : false,
       "message_is_read" : true,
       "message_rand" : 3726251374,
       "message_sender" : "user2",
       "message_seq" : 56858,
       "message_server_time" : 1551101578,
       "message_status" : 2
    },
    "conv_owner" : "",
    "conv_type" : 1,
    "conv_unread_num" : 1
 }
 
```

**示例： 接口 [TIMConvGetConvList](Url_for_TIMCloud#TIMConvGetConvList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [ConvInfo](Url_for_TIMCloudDef#ConvInfo) 。**

```c
 [
    {
       "conv_active_time" : 1551269275,
       "conv_id" : "user2",
       "conv_is_has_draft" : false,
       "conv_is_has_lastmsg" : true,
       "conv_last_msg" : {
          "message_client_time" : 1551235066,
          "message_conv_id" : "user2",
          "message_conv_type" : 1,
          "message_elem_array" : [
             {
                "elem_type" : 0,
                "text_elem_content" : "ccccccccccccccccc"
             }
          ],
          "message_is_from_self" : true,
          "message_is_read" : true,
          "message_rand" : 1073033786,
          "message_sender" : "user1",
          "message_seq" : 16373,
          "message_server_time" : 1551235067,
          "message_status" : 2
       },
       "conv_owner" : "",
       "conv_type" : 1,
       "conv_unread_num" : 0
    }
 ]
 
```

**示例： 接口 [TIMMsgFindByMsgLocatorList](Url_for_TIMCloud#TIMMsgFindByMsgLocatorList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [Message](Url_for_TIMCloudDef#Message) 。**

```c
 [
    {
       "message_client_time" : 1551080111,
       "message_conv_id" : "user2",
       "message_conv_type" : 1,
       "message_elem_array" : [
          {
             "elem_type" : 0,
             "text_elem_content" : "123213213"
          }
       ],
       "message_is_from_self" : true,
       "message_is_read" : true,
       "message_rand" : 2130485001,
       "message_sender" : "user1",
       "message_seq" : 1,
       "message_server_time" : 1551080111,
       "message_status" : 2
    },
    ...
 ]
 
```

**示例： 接口 [TIMMsgGetMsgList](Url_for_TIMCloud#TIMMsgGetMsgList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [Message](Url_for_TIMCloudDef#Message) 。**

```c
 [
    {
       "message_client_time" : 1551080111,
       "message_conv_id" : "user2",
       "message_conv_type" : 1,
       "message_elem_array" : [
          {
             "elem_type" : 0,
             "text_elem_content" : "123213213"
          }
       ],
       "message_is_from_self" : true,
       "message_is_read" : true,
       "message_rand" : 2130485001,
       "message_sender" : "user1",
       "message_seq" : 1,
       "message_server_time" : 1551080111,
       "message_status" : 2
    },
    ...
 ]
```

**示例： 接口 [TIMMsgDownloadElemToPath](Url_for_TIMCloud#TIMMsgDownloadElemToPath) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [MsgDownloadElemResult](Url_for_TIMCloudDef#MsgDownloadElemResult) 。**

```c
 {
   "msg_download_elem_result_current_size" : 10,
   "msg_download_elem_result_total_size" : 100
 }
 
```

**示例： 接口 [TIMMsgBatchSend](Url_for_TIMCloud#TIMMsgBatchSend) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [MsgBatchSendResult](Url_for_TIMCloudDef#MsgBatchSendResult) 。**

```c
 [
    {
       "msg_batch_send_result_code" : 0,
       "msg_batch_send_result_desc" : "",
       "msg_batch_send_result_identifier" : "user3"
    },
    {
       "msg_batch_send_result_code" : 0,
       "msg_batch_send_result_desc" : "",
       "msg_batch_send_result_identifier" : "user2"
    }
 ]
```

**示例： 接口 [TIMGroupCreate](Url_for_TIMCloud#TIMGroupCreate) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [CreateGroupResult](Url_for_TIMCloudDef#CreateGroupResult) 。**

```c
 {
    "create_group_result_groupid" : "first group id"
 }
 
```

**示例： 接口 [TIMGroupInviteMember](Url_for_TIMCloud#TIMGroupInviteMember) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [GroupInviteMemberResult](Url_for_TIMCloudDef#GroupInviteMemberResult) **

```c
 [
    {
       "group_invite_member_result_identifier" : "user2",
       "group_invite_member_result_result" : 1
    },
    {
       "group_invite_member_result_identifier" : "user3",
       "group_invite_member_result_result" : 1
    }
 ]
 
```

**示例： 接口 [TIMGroupDeleteMember](Url_for_TIMCloud#TIMGroupDeleteMember) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [GroupDeleteMemberResult](Url_for_TIMCloudDef#GroupDeleteMemberResult) **

```c
 [
    {
       "group_delete_member_result_identifier" : "user2",
       "group_delete_member_result_result" : 1
    },
    {
       "group_delete_member_result_identifier" : "user3",
       "group_delete_member_result_result" : 1
    }
 ]
 
```

**示例： 接口 [TIMGroupGetJoinedGroupList](Url_for_TIMCloud#TIMGroupGetJoinedGroupList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [GroupBaseInfo](Url_for_TIMCloudDef#GroupBaseInfo) **

```c
 [
    {
       "group_base_info_face_url" : "group face url",
       "group_base_info_group_id" : "first group id",
       "group_base_info_group_name" : "first group name",
       "group_base_info_group_type" : "Public",
       "group_base_info_info_seq" : 7,
       "group_base_info_is_shutup_all" : false,
       "group_base_info_lastest_seq" : 0,
       "group_base_info_msg_flag" : 0,
       "group_base_info_readed_seq" : 0,
       "group_base_info_self_info" : {
          "group_self_info_join_time" : 1551344977,
          "group_self_info_msg_flag" : 0,
          "group_self_info_role" : 400,
          "group_self_info_unread_num" : 0
       }
    }
 ]
```

**示例： 接口 [TIMGroupGetGroupInfoList](Url_for_TIMCloud#TIMGroupGetGroupInfoList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [GetGroupInfoResult](Url_for_TIMCloudDef#GetGroupInfoResult) **

```c
 [
    {
       "get_groups_info_result_code" : 0,
       "get_groups_info_result_desc" : "",
       "get_groups_info_result_info" : {
          "group_detial_info_add_option" : 2,
          "group_detial_info_create_time" : 1551344977,
          "group_detial_info_custom_info" : {},
          "group_detial_info_face_url" : "group face url",
          "group_detial_info_group_id" : "first group id",
          "group_detial_info_group_name" : "first group name",
          "group_detial_info_group_type" : "Public",
          "group_detial_info_info_seq" : 7,
          "group_detial_info_introduction" : "group introduction",
          "group_detial_info_is_shutup_all" : false,
          "group_detial_info_last_info_time" : 1551344977,
          "group_detial_info_last_msg_time" : 0,
          "group_detial_info_max_member_num" : 2000,
          "group_detial_info_member_num" : 1,
          "group_detial_info_next_msg_seq" : 0,
          "group_detial_info_notification" : "group notification",
          "group_detial_info_online_member_num" : 0,
          "group_detial_info_owener_identifier" : "user1",
          "group_detial_info_searchable" : 2,
          "group_detial_info_visible" : 2
       }
    }
 ]
```

**示例： 接口 [TIMGroupGetMemberInfoList](Url_for_TIMCloud#TIMGroupGetMemberInfoList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [GroupGetMemberInfoListResult](Url_for_TIMCloudDef#GroupGetMemberInfoListResult) **

```c
 {
    "group_get_memeber_info_list_result_info_array" : [
       {
          "group_member_info_custom_info" : {},
          "group_member_info_identifier" : "user1",
          "group_member_info_join_time" : 1551344977,
          "group_member_info_member_role" : 400,
          "group_member_info_msg_flag" : 0,
          "group_member_info_msg_seq" : 0,
          "group_member_info_name_card" : "",
          "group_member_info_shutup_time" : 0
       }
    ],
    "group_get_memeber_info_list_result_next_seq" : 0
 }
 
```

**示例： 接口 [TIMGroupGetPendencyList](Url_for_TIMCloud#TIMGroupGetPendencyList) 的回调TIMCommCallback参数json_params的Json。Json Key参考 [GroupPendencyResult](Url_for_TIMCloudDef#GroupPendencyResult)  **

```c
 {
    "group_pendency_result_next_start_time" : 0,
    "group_pendency_result_pendency_array" : [
       {
          "group_pendency_add_time" : 1551414487947,
          "group_pendency_apply_invite_msg" : "Want Join Group, Thank you",
          "group_pendency_approval_msg" : "",
          "group_pendency_form_identifier" : "user2",
          "group_pendency_form_user_defined_data" : "",
          "group_pendency_group_id" : "four group id",
          "group_pendency_handle_result" : 0,
          "group_pendency_handled" : 0,
          "group_pendency_pendency_type" : 0,
          "group_pendency_to_identifier" : "user1",
          "group_pendency_to_user_defined_data" : ""
       }
    ],
    "group_pendency_result_read_time_seq" : 0,
    "group_pendency_result_unread_num" : 1
 }
 
```

**注释： 其他接口的回调TIMCommCallback参数json_params均为空字符串""**

>  [TIMLogin](Url_for_TIMCloud#TIMLogin)   
>  [TIMLogout](Url_for_TIMCloud#TIMLogout)   
>  [TIMMsgSendNewMsg](Url_for_TIMCloud#TIMMsgSendNewMsg)  
>  [TIMMsgSaveMsg](Url_for_TIMCloud#TIMMsgSaveMsg)  
>  [TIMMsgReportReaded](Url_for_TIMCloud#TIMMsgReportReaded)  
>  [TIMMsgRevoke](Url_for_TIMCloud#TIMMsgRevoke)  
>  [TIMMsgImportMsgList](Url_for_TIMCloud#TIMMsgImportMsgList)  
>  [TIMMsgDelete](Url_for_TIMCloud#TIMMsgDelete)  
>  [TIMConvDelete](Url_for_TIMCloud#TIMConvDelete)  
>  [TIMGroupDelete](Url_for_TIMCloud#TIMGroupDelete)  
>  [TIMGroupJoin](Url_for_TIMCloud#TIMGroupJoin)  
>  [TIMGroupQuit](Url_for_TIMCloud#TIMGroupQuit)  
>  [TIMGroupSetGroupInfo](Url_for_TIMCloud#TIMGroupSetGroupInfo)  
>  [TIMGroupSetMemberInfo](Url_for_TIMCloud#TIMGroupSetMemberInfo)  
>  [TIMGroupReportPendencyReaded](Url_for_TIMCloud#TIMGroupReportPendencyReaded)  
>  [TIMGroupHandlePendency](Url_for_TIMCloud#TIMGroupHandlePendency) 




