api-best-practices
==================

### Authentication

- OAuth 2.0 を使う。

### Content-Type

- API は Accept-Content-Type に応じた Content-Type でレスポンスを返却すべきである。
  - よく用いられるのは `application/json`, `application/javascript` (JSONP), `application/x-msgpack`, `application/x-plist` 等

### Entity Representation

- レスポンスは、Entity をその snake_case をキーにした Object をルートオブジェクトにすべきである。
  - **Entity をルートにしない**
  - これは、後々他の情報を埋め込んで拡張するため。
- キーは snake_case が推奨される。

```json
{
    "book": {
        "title": "Alice's Adventures in Wonderland"
    }
}
```

### Pagination

`cursor: string` をパラメータに取る。

```json
{
    "books": [
        ...
    ],
    "pagination": {
        "previous_cursor": "7dsajf9j2f9",
        "next_cursor": "9sfjlsa9fa9"
    }
}
```

### Hyperschema

- API は自身を記述した Hyperschema を提供すべきである。(`GET /schema` 等)
- JSON Schema が良く用いられる。

### Error Handling

- エラーレスポンスは、適切な HTTP Status Code を返す
- エラーレスポンスは、その Body に、固定されたフォーマットでエラーを記述する。

```json
{
    "error": {
        "code": 1788,
        "kind": "some/error",
        "description": "You have some error!",
        "url": "http://example.com/path/to/docoument"
    }
}
```

- **扱いに難があるので `text/plain` でエラーテキストを返さない**

### Documentation

- ドキュメントは、Hyperschema を用いて生成することが推奨される。
- ドキュメントは、レスポンスの生じうる Error Response を表明すべきである。
  - **Error Response は Exception に等しい。**

### Request ID

- API は全てのリクエストに対して、一意の request id を UUID 形式で生成し、Response Header に含めることが推奨される。(`X-Request-ID` など)

Advanced
---

### Representations

#### Patch

- Patch (Delta) は JSON Patch を用いて表現する。

### Batch Request

- API は、バッチリクエストの方法を提供すべきである。そのための方法として、JSON RPC 2.0 Batch がある。
