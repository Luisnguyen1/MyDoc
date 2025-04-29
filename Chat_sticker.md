# Luồng gửi Sticker trong Room Chat

## 1. Luồng hoạt động

1. Client gọi API lấy danh sách sticker
2. Người dùng chọn sticker muốn gửi
3. Client kết nối WebSocket với room_id và token
4. Client gửi message với type STICKER và sticker_id qua WebSocket
5. Server xử lý và lưu tin nhắn sticker
6. Client có thể:
   - Lấy tin nhắn mới bằng API chi tiết tin nhắn
   - Xem danh sách room với tin nhắn mới nhất

## 2. Chú ý

- Token cần được include trong header hoặc query params cho tất cả request
- WebSocket connection cần được duy trì để nhận tin nhắn realtime
- Sticker_id phải là một ID hợp lệ từ danh sách sticker
- Response của các API sẽ bao gồm thông tin về sticker trong tin nhắn

## 3. Lấy danh sách Sticker

### Request
```http
GET {{domain}}/api/v1/general/sticker/
```

### Response
```json
{
    "status_code": 200,
    "message": "Danh sách sticker",
    "data": [
        {
            "id": "6a64d73b-c2fb-44f4-96f9-546b3cb40847",
            "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/007.gif"
        },
        {
            "id": "c3047c7e-16ea-4aa8-acc4-bef1e3bd7ee0",
            "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/016.gif"
        },
        {
            "id": "a7d62141-2a23-48e2-a651-d7deab60247d",
            "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/008_1.gif"
        },
        {
            "id": "59008625-cdee-4080-ae81-31e54aefc671",
            "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/015.gif"
        },
        {
            "id": "42812260-fb8d-4964-a1ef-4e1c18021572",
            "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/012.gif"
        }
    ]
}
```

## 2. Kết nối WebSocket và gửi Sticker

### WebSocket Connection Requirements

#### 1. Connection URL
```
ws://localhost:8006/conversation/?room_id={ROOM_ID}&token={TOKEN}
```

#### 2. Required Query Parameters
```
room_id: String (example: "644cfdc9-e297-4468-aead-6cc154a132ed")
token: String (JWT token for authentication)
```

#### 3. Required Headers
```
Origin: http://localhost:8006/
Connection: Upgrade
Upgrade: websocket
```

### Message Format để gửi Sticker
```json
{
    "type": "STICKER",
    "message": {
        "type": "STICKER",
        "sticker_id": "6a64d73b-c2fb-44f4-96f9-546b3cb40847",
        "room_id": "644cfdc9-e297-4468-aead-6cc154a132ed"
    }
}
```

## 3. API lấy danh sách tin nhắn

### 3.1. API lấy chi tiết tin nhắn trong room

#### Request
```http
GET {{domain}}/api/v1/conversation/detail/room/message/644cfdc9-e297-4468-aead-6cc154a132ed/?limit=3&offset=0
```

#### Response Example
```json
{
    "status_code": 200,
    "message": "Danh sách tin nhắn",
    "data": {
        "count": 41,
        "next": "http://localhost:8006/api/v1/conversation/detail/room/message/644cfdc9-e297-4468-aead-6cc154a132ed/?limit=3&offset=3",
        "previous": null,
        "results": [
            {
                "id": "5b782792-c1d7-4cf6-94f1-a882cc104bad",
                "reply_to": null,
                "forwarded_from": null,
                "sticker": {
                    "id": "6a64d73b-c2fb-44f4-96f9-546b3cb40847",
                    "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/007.gif"
                },
                "gift": null,
                "type": "STICKER",
                "text": "Sơn Tùng đã gửi một nhãn dán",
                "is_revoked": false,
                "is_seen": false,
                "is_edited": false,
                "is_pinned": false,
                "created_at": "2025-04-29T09:39:25.314671",
                "update_at": "2025-04-29T09:39:25.325828+07:00",
                "location": null,
                "style_text": null,
                "share_data": null,
                "sender": {
                    "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                    "full_name": "Sơn Tùng",
                    "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                    "is_online": false,
                    "is_private": false,
                    "setting_private": {
                        "hide_location": false,
                        "prevent_add_room": false,
                        "hide_match_friend": false,
                        "prevent_add_group": false,
                        "prevent_call_another": false,
                        "hide_recommend_friend": false,
                        "receive_message_random": true,
                        "receive_message_another": true
                    },
                    "avatar_frame": null,
                    "is_vip": false,
                    "setting_vip": {
                        "hide_age": false,
                        "hide_gender": false,
                        "hide_location": false,
                        "hide_avt_frame": false,
                        "prevent_search": false
                    }
                },
                "room": "644cfdc9-e297-4468-aead-6cc154a132ed",
                "call": null,
                "file": [],
                "seen_by": [
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "date_seen": "2025-04-29T09:39:32.873092+07:00"
                    }
                ]
            },
            {
                "id": "809bb016-4dbd-4ec4-b4be-ce10e2acf965",
                "reply_to": null,
                "forwarded_from": null,
                "sticker": {
                    "id": "6a64d73b-c2fb-44f4-96f9-546b3cb40847",
                    "file": "https://minio-dev.occo.vn/tok-uat-bucket/assets/stickers/007.gif"
                },
                "gift": null,
                "type": "STICKER",
                "text": "Sơn Tùng đã gửi một nhãn dán",
                "is_revoked": false,
                "is_seen": false,
                "is_edited": false,
                "is_pinned": false,
                "created_at": "2025-04-29T09:39:08.125651",
                "update_at": "2025-04-29T09:39:08.130662+07:00",
                "location": null,
                "style_text": null,
                "share_data": null,
                "sender": {
                    "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                    "full_name": "Sơn Tùng",
                    "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                    "is_online": false,
                    "is_private": false,
                    "setting_private": {
                        "hide_location": false,
                        "prevent_add_room": false,
                        "hide_match_friend": false,
                        "prevent_add_group": false,
                        "prevent_call_another": false,
                        "hide_recommend_friend": false,
                        "receive_message_random": true,
                        "receive_message_another": true
                    },
                    "avatar_frame": null,
                    "is_vip": false,
                    "setting_vip": {
                        "hide_age": false,
                        "hide_gender": false,
                        "hide_location": false,
                        "hide_avt_frame": false,
                        "prevent_search": false
                    }
                },
                "room": "644cfdc9-e297-4468-aead-6cc154a132ed",
                "call": null,
                "file": [],
                "seen_by": [
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "date_seen": "2025-04-29T09:39:32.740051+07:00"
                    }
                ]
            },
            {
                "id": "fab6fe14-e851-442d-8ccc-b156732609c5",
                "reply_to": null,
                "forwarded_from": null,
                "sticker": null,
                "gift": null,
                "type": "TEXT",
                "text": "xin chào anh Tây!",
                "is_revoked": false,
                "is_seen": false,
                "is_edited": false,
                "is_pinned": false,
                "created_at": "2025-04-15T14:41:09.888414",
                "update_at": "2025-04-15T14:41:09.888461+07:00",
                "location": null,
                "style_text": null,
                "share_data": null,
                "sender": {
                    "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                    "full_name": "Sơn Tùng",
                    "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                    "is_online": false,
                    "is_private": false,
                    "setting_private": {
                        "hide_location": false,
                        "prevent_add_room": false,
                        "hide_match_friend": false,
                        "prevent_add_group": false,
                        "prevent_call_another": false,
                        "hide_recommend_friend": false,
                        "receive_message_random": true,
                        "receive_message_another": true
                    },
                    "avatar_frame": null,
                    "is_vip": false,
                    "setting_vip": {
                        "hide_age": false,
                        "hide_gender": false,
                        "hide_location": false,
                        "hide_avt_frame": false,
                        "prevent_search": false
                    }
                },
                "room": "644cfdc9-e297-4468-aead-6cc154a132ed",
                "call": null,
                "file": [],
                "seen_by": [
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "date_seen": "2025-04-29T09:24:46.120061+07:00"
                    }
                ]
            }
        ]
    }
}
```

### 3.2. API lấy danh sách room với tin nhắn mới nhất

#### Request
```http
GET {{domain}}/api/v1/conversation/list/room-test2/?type=CONNECT&limit=20&offset=0&message_limit=20
```

#### Response Example
```json
{
    "status_code": 200,
    "message": "Danh sách phòng và tin nhắn",
    "data": {
        "count": 5,
        "next": null,
        "previous": null,
        "results": [
            {
                "id": "e7e31ae6-6491-4dcc-9e3e-b01a73aaefcd",
                "type": "CSKH",
                "name": "Chăm sóc khách hàng",
                "newest_at": null,
                "created_at": "2025-04-11T12:13:25.103385",
                "image": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                "background_image": null,
                "background_color": null,
                "block_status": null,
                "notification_mode": "on",
                "last_message": {},
                "total_unseen": 0,
                "list_users": [
                    {
                        "user": {
                            "id": "4eead0e7-b65d-4524-b7bb-cfa52eaafba4",
                            "full_name": "Chăm sóc khách hàng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-11T12:13:25.150305+07:00",
                        "nickname": null,
                        "code": null
                    },
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-11T12:13:25.149892+07:00",
                        "nickname": null,
                        "code": null
                    }
                ],
                "total_users": 2,
                "is_pinned_msg": false,
                "messages": []
            },
            {
                "id": "644cfdc9-e297-4468-aead-6cc154a132ed",
                "type": "CONNECT",
                "name": "Tester",
                "newest_at": "2025-04-29T09:39:25.329940+07:00",
                "created_at": "2025-04-15T10:51:50.240353",
                "image": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                "background_image": null,
                "background_color": null,
                "block_status": null,
                "notification_mode": "on",
                "last_message": {
                    "id": "5b782792-c1d7-4cf6-94f1-a882cc104bad",
                    "text": "Sơn Tùng đã gửi một nhãn dán",
                    "created_at": "2025-04-29T09:39:25.314671"
                },
                "total_unseen": 0,
                "list_users": [
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-15T10:51:50.245466+07:00",
                        "nickname": null,
                        "code": null
                    },
                    {
                        "user": {
                            "id": "da904428-0b4c-410f-ad13-17d64ee78abb",
                            "full_name": "Tester",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-15T10:51:50.245057+07:00",
                        "nickname": null,
                        "code": null
                    }
                ],
                "total_users": 2,
                "is_pinned_msg": false,
                "messages": [
                    {
                        "id": "5b782792-c1d7-4cf6-94f1-a882cc104bad",
                        "text": "Sơn Tùng đã gửi một nhãn dán",
                        "type": "STICKER",
                        "created_at": "2025-04-29T09:39:25.314671+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "809bb016-4dbd-4ec4-b4be-ce10e2acf965",
                        "text": "Sơn Tùng đã gửi một nhãn dán",
                        "type": "STICKER",
                        "created_at": "2025-04-29T09:39:08.125651+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "fab6fe14-e851-442d-8ccc-b156732609c5",
                        "text": "xin chào anh Tây!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:41:09.888414+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "e287543f-4a70-46e8-95ce-3878cdd8db08",
                        "text": "chao anh Tay",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:40:59.208013+07:00",
                        "sender_id": "da904428-0b4c-410f-ad13-17d64ee78abb"
                    },
                    {
                        "id": "f84491cf-5958-44ec-9c11-f9df6f4d0166",
                        "text": "xin chào anh Tây!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:40:45.634397+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "06f9c71f-7cf3-468a-8385-60619fe6b6a9",
                        "text": "xin chào aaax!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:40:23.646210+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "7418d39c-ce1f-4bdf-b49f-736f085fb189",
                        "text": "xin chào aaax!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:40:09.750663+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "66cefd9e-e24a-4732-b8e2-aff7f7d4e9fd",
                        "text": "xin chao",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:39:40.665120+07:00",
                        "sender_id": "da904428-0b4c-410f-ad13-17d64ee78abb"
                    },
                    {
                        "id": "7a8ac367-b1a9-4cd6-8a28-7745c411f7ee",
                        "text": "xin chào aaax!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:39:34.602559+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "ae22a6f0-c242-4b8e-ae29-f8c094767249",
                        "text": "xin chào aaax!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:39:31.443389+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "bdd295d6-d6be-4fc0-b563-6ccfd0f2efca",
                        "text": "xin chào aaax!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:39:04.677679+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "bb8bc674-1024-4a95-b104-b853cdde5fdf",
                        "text": "xin chào aaax!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:38:59.128112+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "bdbd499a-bad3-460f-9029-bbacbdb9d76f",
                        "text": "xin chào an!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:38:45.647114+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "f9ffe4f2-eec8-4472-8a50-eae13d440af4",
                        "text": "xin chào Phuc!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:38:41.231817+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "026064dd-eb7a-458e-a8ef-864acd8819c1",
                        "text": "xin chào Phuc!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:38:32.592992+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "156a7b01-933f-4a4e-b107-8ded9ac9d288",
                        "text": "xin chào Phuc!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:37:57.159566+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "a4058973-45d2-4b53-bb79-da9c25a700d8",
                        "text": "xin chào!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:32:12.985566+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "3e3b7df9-974a-44e1-a61b-a64829c5391c",
                        "text": "xin chào!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:28:30.131722+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "382f0bac-a29e-4d95-9ffd-ee5cb38a5387",
                        "text": "xin chào!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:21:25.742938+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "0970f057-4da4-4b81-af04-7be9b6608382",
                        "text": "xin chao",
                        "type": "TEXT",
                        "created_at": "2025-04-15T14:21:11.808878+07:00",
                        "sender_id": "da904428-0b4c-410f-ad13-17d64ee78abb"
                    }
                ]
            },
            {
                "id": "83353786-3a1a-4c00-9f2d-2daec2a9e329",
                "type": "CONNECT",
                "name": "Phuc Dev",
                "newest_at": "2025-04-15T10:50:16.458867+07:00",
                "created_at": "2025-04-15T10:48:17.879213",
                "image": "https://minio-dev.occo.vn/tok-uat-bucket/user_media/9f7698aa-a2cd-4c4f-b8c8-d632a7472c77/media/599ad2f8-ff84-486c-b787-b84f7d4d328b.jpg",
                "background_image": null,
                "background_color": null,
                "block_status": null,
                "notification_mode": "on",
                "last_message": {
                    "id": "9860a2a2-ccd8-474f-9e7e-a23f2afd0b91",
                    "text": "Xin chào Phúc",
                    "created_at": "2025-04-15T10:50:16.455756"
                },
                "total_unseen": 0,
                "list_users": [
                    {
                        "user": {
                            "id": "9f7698aa-a2cd-4c4f-b8c8-d632a7472c77",
                            "full_name": "Phuc Dev",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/user_media/9f7698aa-a2cd-4c4f-b8c8-d632a7472c77/media/599ad2f8-ff84-486c-b787-b84f7d4d328b.jpg",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-15T10:48:17.883969+07:00",
                        "nickname": null,
                        "code": null
                    },
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-15T10:48:17.884352+07:00",
                        "nickname": null,
                        "code": null
                    }
                ],
                "total_users": 2,
                "is_pinned_msg": false,
                "messages": [
                    {
                        "id": "9860a2a2-ccd8-474f-9e7e-a23f2afd0b91",
                        "text": "Xin chào Phúc",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:50:16.455756+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "30e7149f-cb80-44bf-beda-50d9f46e8b31",
                        "text": "Xin chào Phúc",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:50:01.465585+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "161e4ca1-0981-43b7-b37f-42e5cfc7687c",
                        "text": "Xin chào Phúc",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:49:50.283873+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "d7b16238-ea1a-49e3-b1ca-3c8bc87fc1e1",
                        "text": "Xin chào Phúc",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:49:33.521946+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "4c43b76e-1823-4058-8851-f9e884ee10a1",
                        "text": "Xin chào Bitaasdasdasdas ebasdaasdasddsdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:48:56.322600+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "f3be3dd8-97f8-4298-97d3-9bd819356c58",
                        "text": "Xin chào Bitaasdasdasdas ebasdaasdasddsdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:48:52.059842+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "4a827503-a82d-43de-b47f-6de02d9b6892",
                        "text": "Xin chào Bitaasdasdasdas ebasdaasdasddsdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:48:44.435429+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "dc6678a3-7062-475a-a3d5-a33fc3230b9f",
                        "text": "Xin chào Phúc",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:48:37.866297+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    }
                ]
            },
            {
                "id": "9cf9fc31-6ab0-4032-8cae-c14b5f5e26ac",
                "type": "CONNECT",
                "name": "Tester",
                "newest_at": "2025-04-15T10:46:59.748396+07:00",
                "created_at": "2025-04-15T10:44:37.180280",
                "image": "https://minio-dev.occo.vn/tok-uat-bucket/user_media/a8046945-e2a8-4b8c-b006-743f7233a244/media/e85abb45-f4ff-4f4d-9bbc-03cca31c6a20.jpg",
                "background_image": null,
                "background_color": null,
                "block_status": null,
                "notification_mode": "on",
                "last_message": {
                    "id": "0ef0914a-e9a9-406a-98f2-222e7c51d00a",
                    "text": "Xin chào Bitaasdasdasdas ebasdaasdasddsdaa!",
                    "created_at": "2025-04-15T10:46:59.744520"
                },
                "total_unseen": 0,
                "list_users": [
                    {
                        "user": {
                            "id": "a8046945-e2a8-4b8c-b006-743f7233a244",
                            "full_name": "Tester",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/user_media/a8046945-e2a8-4b8c-b006-743f7233a244/media/e85abb45-f4ff-4f4d-9bbc-03cca31c6a20.jpg",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-15T10:44:37.185009+07:00",
                        "nickname": null,
                        "code": null
                    },
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-15T10:44:37.185390+07:00",
                        "nickname": null,
                        "code": null
                    }
                ],
                "total_users": 2,
                "is_pinned_msg": false,
                "messages": [
                    {
                        "id": "0ef0914a-e9a9-406a-98f2-222e7c51d00a",
                        "text": "Xin chào Bitaasdasdasdas ebasdaasdasddsdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:46:59.744520+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "1bf3addb-74e8-4fde-96f3-b276f7de047b",
                        "text": "Xin chào Bitaasdasdasdas ebasdasdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:46:46.150526+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "839d4a86-7e27-42cf-a2c7-b80dcbe6b0b9",
                        "text": "Xin chào Phúc",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:45:58.370754+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "326cdf4d-dd2f-4424-aef5-54f1a633237a",
                        "text": "Xin chào Bitaasdasdasdas ebasdasdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:45:49.781380+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "6f7244b2-61e6-419f-8627-4e6f909da6af",
                        "text": "Xin chào Bitaasdasdasdas ebasdasdaa!",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:45:31.110709+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "cfc796aa-bfbb-49b5-b4dd-9d8778abba26",
                        "text": "Xin chào Phúc",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:45:20.838723+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "d5a67fd1-a0b8-4e36-8527-f446da4953ad",
                        "text": "Xin chào Bitaasdasdasdas ebasdasdaa!",
                        "type": "TEXT",
                        "created_at": "2025-04-15T10:45:07.076084+07:00",
                        "sender_id": "317b287a-8ba4-4a46-b70d-4ebbedbd03ff"
                    },
                    {
                        "id": "b0c80aa3-b7a9-4638-b621-a013146c1d6b",
                        "text": "Xin chào Phúc",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:44:58.932663+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    }
                ]
            },
            {
                "id": "1cfc51d1-532d-4b04-96b2-e0ef8c61ede1",
                "type": "CONNECT",
                "name": "Lệ Quyên",
                "newest_at": "2025-04-15T10:41:24.890524+07:00",
                "created_at": "2025-04-11T13:07:41.775658",
                "image": "https://minio-dev.occo.vn/tok-uat-bucket/user_media/cc45ecaa-84f7-448d-a31c-17db061b54e7/media/0bb12793-d69f-4985-9a0f-86f770fb8b5b.jpg",
                "background_image": null,
                "background_color": null,
                "block_status": null,
                "notification_mode": "on",
                "last_message": {
                    "id": "82cc044e-43c7-4a6e-a3d7-20d3276fc3a9",
                    "text": "Hello 1",
                    "created_at": "2025-04-15T10:41:24.867756"
                },
                "total_unseen": 0,
                "list_users": [
                    {
                        "user": {
                            "id": "cc45ecaa-84f7-448d-a31c-17db061b54e7",
                            "full_name": "Lệ Quyên",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/user_media/cc45ecaa-84f7-448d-a31c-17db061b54e7/media/0bb12793-d69f-4985-9a0f-86f770fb8b5b.jpg",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-11T13:07:41.779285+07:00",
                        "nickname": null,
                        "code": null
                    },
                    {
                        "user": {
                            "id": "0e63e901-06f8-411e-bee1-52a1872ede13",
                            "full_name": "Sơn Tùng",
                            "avatar": "https://minio-dev.occo.vn/tok-uat-bucket/assets/default/avatar/default_avatar.png",
                            "is_online": false,
                            "is_private": false,
                            "setting_private": {
                                "hide_location": false,
                                "prevent_add_room": false,
                                "hide_match_friend": false,
                                "prevent_add_group": false,
                                "prevent_call_another": false,
                                "hide_recommend_friend": false,
                                "receive_message_random": true,
                                "receive_message_another": true
                            },
                            "avatar_frame": null,
                            "is_vip": false,
                            "setting_vip": {
                                "hide_age": false,
                                "hide_gender": false,
                                "hide_location": false,
                                "hide_avt_frame": false,
                                "prevent_search": false
                            }
                        },
                        "role": null,
                        "last_active": "2025-04-11T13:07:41.779499+07:00",
                        "nickname": null,
                        "code": null
                    }
                ],
                "total_users": 2,
                "is_pinned_msg": false,
                "messages": [
                    {
                        "id": "82cc044e-43c7-4a6e-a3d7-20d3276fc3a9",
                        "text": "Hello 1",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:41:24.867756+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "6fa0fb78-8eba-436e-b85a-65f7a7a34e1d",
                        "text": "Hello 1",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:41:07.363218+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "aa9beb4d-4786-440a-8c49-e78b29773f5e",
                        "text": "Hello 1",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:59.044919+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "45b7eb35-71a8-4e70-bdd5-007daee21d14",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:33.112844+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "388f0895-f343-4b66-a2d4-dc20e8c9a5e8",
                        "text": "Hello bà",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:25.775359+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "95881571-178b-401e-9583-a3373cb976c8",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:23.244765+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "d2198a03-9e32-4c34-9c29-a95165bb074d",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:21.886356+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "929eefaf-b66b-47ab-b2f1-b79621d5d6c0",
                        "text": "Hello bà",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:19.200427+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "55d3dcfb-254d-4278-a50b-b68a74143bf0",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:40:14.596639+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "c048cf01-990d-407d-b707-8177cc7b1488",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:39:48.240140+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "1701285d-3118-47c1-bbf6-dac2c1e69a80",
                        "text": "Hello bà",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:39:38.441094+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "47a6f662-77de-491f-b622-d84b3c707fc4",
                        "text": "Hello bà",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:39:12.758621+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "93ad205c-85fd-4efb-bfa9-75dcb45100c5",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:39:02.800351+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "ad6b2838-7589-4b41-bbb7-e7a23d613b88",
                        "text": "Hello bà",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:36:06.422210+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "58eaf75b-1382-48a3-bb87-ed036cdf4de4",
                        "text": "Hello ông",
                        "type": "IMAGE",
                        "created_at": "2025-04-15T10:35:57.949677+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "99e494d2-1087-4b6a-a01e-17efb4308ff8",
                        "text": "Son Tung nhan Le QUyen1",
                        "type": "TEXT",
                        "created_at": "2025-04-11T13:36:08.579120+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "fb2aca47-8198-42de-82f3-d23c9865d340",
                        "text": "Son Tung nhan Le QUyen1",
                        "type": "TEXT",
                        "created_at": "2025-04-11T13:35:58.384956+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "a9b12a76-734b-4247-81d4-da7fb53f6e47",
                        "text": "Le Quyen nhan Son Tung",
                        "type": "TEXT",
                        "created_at": "2025-04-11T13:35:55.316004+07:00",
                        "sender_id": "cc45ecaa-84f7-448d-a31c-17db061b54e7"
                    },
                    {
                        "id": "79dcd9f7-16ae-4696-b172-8131080466bf",
                        "text": "Son Tung nhan Le QUyen1",
                        "type": "TEXT",
                        "created_at": "2025-04-11T13:30:31.746727+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    },
                    {
                        "id": "727dd319-1cd6-44a0-bd7c-7d0b7692beb2",
                        "text": "Son Tung nhan Le QUyen1",
                        "type": "TEXT",
                        "created_at": "2025-04-11T13:30:28.920010+07:00",
                        "sender_id": "0e63e901-06f8-411e-bee1-52a1872ede13"
                    }
                ]
            }
        ]
    }
}
```


