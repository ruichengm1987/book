# restapi规范

## 规范
### URI
#### 设计原则
* 资源必须使用复数名词表示集合，如果该词语没有合适的复数形式，则应该使用单数形式
```aidl
例如：
  GET /employees
  GET /weather
  
必须使用正斜杠（/）表示层次关系
正斜杠（/）字符用于URI的路径部分，以指示资源之间的层次关系。
例如：
   GET /employees/56
```
* 不应该在URI中使用尾部正斜杠（/）

* 应该使用连字符（ - ）来提高URI的可读性，不应该使用下划线（ _ ）

* 必须在URI中使用小写字母

* 资源包含父子嵌套关系必须遵循以下原则  
```aidl
如果该资源脱离父资源后没有意义，则如下设计，例如：文件的某行与文件的关系
  GET /file/1000/line/20
如果该资源可以独立访问或可以从属于多个父资源，则不用嵌套显示，
例如：专辑和歌曲的关系
  GET /albums/151
  GET /songs/10
```
#### 版本控制
由于一个API服务可以提供多个API接口，如果有不兼容和破坏性的更改，版本号将让你能更容易的发布API。版本控制格式约定为：vN（N=1,2,3...）
```aidl
例如:
GET /api/v1/employees/123
GET /api/v2/employees/123
```
### 资源操作
#### HTTP方法
资源操作必须尽可能使用正确的HTTP方法，并且必须遵守操作幂等性。HTTP方法通常被称为HTTP动词。

| 方法 | 安全 | 幂等 |
| :-----| ----: | :----: |
| GET | 是 | 是 |
| POST | 否 | 否 |
| PUT | 否 | 是 |
| DELETE | 否 | 是 |
| PATCH | 否 | 否 |
| OPTIONS | 是 | 是 |
| HEAD | 是 | 是 |

关于以上方法的说明：  
* GET 用于从服务器获取某个资源的信息  
    - 完成请求后返回状态码 200 OK
    - 完成请求后需要返回被请求的资源详细信息
* POST 用于创建新资源
    - 创建完成后返回状态码 201 Created
    - 完成请求后需要返回被创建的资源详细信息
* PUT 用于完整的替换资源，比如修改 id 为 123 的某个资源
    - 如果是替换了资源，则返回 200 OK
    - 完成请求后需要返回被修改的资源详细信息
* PATCH 用于局部更新资源
    - 完成请求后返回状态码 200 OK
    - 完成请求后需要返回被修改的资源详细信息
* DELETE 用于删除某个资源
    - 完成请求后返回状态码 204 No Content
* OPTIONS 用于获取资源支持的所有 HTTP 方法
* HEAD 用于只获取请求某个资源返回的头信息
#### 自定义动词
实际业务场景中，对资源对象的控制，如升级、上传、移动等操作，用以上规范约束并不能保证API语义的唯一性、可理解性、易用性，因此，允许使用自定义动词去显示表达API的语义，统一格式为：
```aidl
/{prefix}/{version}/{resources}:{action}
```
动词命名请参照自定义动词命名规范
例如，文件导入导出：
```aidl
/v1/some/resource:import
/v1/some/resource:export
```
除此之外，撤销操作则使用cancel关键字与该动词用英文连字符-相连。  
例如，停止导入：  
```aidl
/v1/some/resource/name:import-cancel
```
### 请求约束
#### 请求约束
* GET, DELETE, HEAD 方法，参数风格必须为标准的 GET 风格的参数，如 ?a=1&b=2
* POST, PUT, PATCH, OPTIONS 方法 默认情况下请求实体会被视作标准 json 字符串进行处理，应该设置头信息的 Content-Type 为 application/json，在一些特殊接口中，可以允许 Content-Type 为 application/x-www-form-urlencoded 或者 multipart/form-data ，此时请求实体会被视作标准 POST 风格的参数进行处理
* 在特殊场景下，例如查询参数长度超过浏览器限制、不允许使用DELETE方法等，可以使用POST方法代替
* 使用过滤查询时，基本查询和高级查询必须不能同时使用

#### 参数命名
* 参数定义必须使用 lower_case_underscore_separated_names 格式
* 参数名称必须是英文或英文和数字的组合
* 参数名称不应该包含介词（例如for、during、at）
```例如：
reason_for_error 应该改成 error_reason
cpu_usage_at_time_of_failure 应该改成 failure_time_cpu_usage
```
* 参数名称不应该使用后置形容词（名词后面的修饰符
```
例如：
items_collected 应该改成 collected_items
objects_imported 应该改成 imported_objects
```
### 响应约束
#### 响应约束  
客户端请求头如果没有提供Accept，默认的响应结果格式应该是application/json，否则应该使用请求头中Accept指定的格式响应。

响应结果必须使用HTTP标准响应码
* 成功，响应数据为资源本身的描述
* 失败，响应数据为错误描述，且必须包含error对象
响应示例

#### 调用成功
```aidl
HTTP/1.1 200 OK
Content-Type: application/json
{
    "total": 910,
    "items":[{}, {} ...]
}
```
调用失败，详见错误示例

#### 错误
如果是一个不支持的请求或者请求失败，服务必须提供一个错误响应结果，这个错误响应结果必须是一个标准的HTTP 错误码。
#### 响应示例
```aidl
错误响应信息必须使用 error 字段，例如：

```
### 数据类型
### 安全

## 特殊场景
### 分页
### 过滤
### 排序
### 跨域