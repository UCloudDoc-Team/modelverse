# Gemini 批量任务 API

本文档描述通过 ModelVerse 创建、查询与下载 **Gemini（Vertex 风格）批量预测任务**的接口，适用于大批量异步文本生成场景。

---
**当前支持的模型 ID**（创建任务时 `model` 字段仅支持以下三种）：

| 模型 ID |
|--------|
| `publishers/google/models/gemini-3.1-pro-preview` |
| `publishers/google/models/gemini-3-flash-preview` |
| `publishers/google/models/gemini-3-pro-image-preview` |
---

## 认证说明

所有接口支持以下任一方式携带 API Key（请勿在文档或代码中写真实 Key，使用环境变量或配置占位符）：

- `Authorization: Bearer <your_api_key>`
- `x-goog-api-key: <your_api_key>`

Base URL 示例：`https://api.modelverse.cn`（海外可用 `https://api.umodelverse.ai`）。

---

## 流程概览

1. **上传输入文件**：将 JSONL 输入文件上传到平台存储（GCS），获得文件标识。
2. **创建批量任务**：提交批量任务，指定模型、输入文件 URI、输出目录前缀。
3. **查询任务状态**：轮询任务状态，直至完成或失败。
4. **下载结果文件**：任务完成后，根据输出目录下载 `predictions.jsonl`。

---

## 1. 上传批量任务输入文件

将批量任务的输入文件（JSONL）上传到平台，用于后续创建任务时作为输入源。

**请求**

- **方法 / 路径**：`POST /v1/files`
- **Content-Type**：`multipart/form-data`
- **必填字段**：
  - `purpose`：固定为 `**batch:gcs`**（用于 Gemini 批量任务时必须填写）。
  - `file`：输入文件（JSONL）。

**响应**

返回 OpenAI 风格文件对象，主要字段：


| 字段         | 类型     | 说明                |
| ---------- | ------ | ----------------- |
| id         | string | 文件在存储中的对象名        |
| object     | string | 固定为 `"file"`      |
| bytes      | int64  | 文件大小（字节）          |
| created_at | int64  | 创建时间戳             |
| purpose    | string | 即请求中的 `batch:gcs` |


**与创建任务的约定**：上传接口返回的 **id** 为文件在 GCS 中的对象名。创建批量任务时，`inputConfig.gcsSource.uris` 需填写**完整 GCS URI**，格式为：`**gs://<bucket>/<id>`**，其中 `<bucket>` 由平台为批量任务分配，以控制台或当前环境说明为准（例如 `gs://gemini-batch-001/<id>`）。不可使用数组，仅支持字符串形式的单个 URI。

**示例**

```bash
curl -X POST "https://api.modelverse.cn/v1/files" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -F "purpose=batch:gcs" \
  -F "file=@your-input.jsonl"
```
**返回**
```shell
{
  "id": "1773236148797418904_gemini-3.1-pro-preview.jsonl",
  "object": "file",
  "bytes": 224,
  "created_at": 1773236150,
  "expires_at": 0,
  "filename": "1773236148797418904_gemini-3.1-pro-preview.jsonl",
  "purpose": "batch:gcs"
}

```
---

## 2. 创建批量任务

**请求**

- **方法 / 路径**：`POST /v1beta/batchPredictionJobs`
- **Content-Type**：`application/json`

**请求体（仅列与 Gemini 批量任务相关字段）**


| 字段                                          | 类型     | 必填  | 说明                                                                                   |
| ------------------------------------------- | ------ | --- | ------------------------------------------------------------------------------------ |
| displayName                                 | string | 否   | 任务展示名称                                                                               |
| model                                       | string | 是   | 模型资源名，如 `publishers/google/models/gemini-3.1-pro-preview`                            |
| inputConfig.instancesFormat                 | string | 是   | 输入格式，如 `jsonl`                                                                       |
| inputConfig.gcsSource.uris                  | string | 是   | 输入文件 GCS URI（**字符串**），bucket 必须为gemini-batch-001，即 `gs://gemini-batch-001/<上传返回的id>` |
| outputConfig.predictionsFormat              | string | 是   | 输出格式，如 `jsonl`                                                                       |
| outputConfig.gcsDestination.outputUriPrefix | string | 是   | 输出目录前缀，必须指定 `gs://gemini-batch-001/output`                                           |


**响应**

返回 Vertex 风格任务对象。成功创建时，响应中会包含 `name` 字段（可能为长资源名）。服务会将其中最后一段作为 **batch_id / job_id** 供后续查询与下载使用；客户端可按 `name` 或服务返回的简短 id 作为任务唯一标识。

**示例**

```bash
curl -X POST "https://api.modelverse.cn/v1beta/batchPredictionJobs" \
  -H "X-Goog-Api-Key: $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "displayName": "my-cloud-storage-batch-inference-job",
    "model": "publishers/google/models/gemini-3-flash-preview",
    "inputConfig": {
      "instancesFormat": "jsonl",
      "gcsSource": {
        "uris": "gs://gemini-batch-001/<上传返回的id>"
      }
    },
    "outputConfig": {
      "predictionsFormat": "jsonl",
      "gcsDestination": {
        "outputUriPrefix": "gs://gemini-batch-001/output"
      }
    }
  }'
```

**返回**
```json
{
  "name": "projects/279887244472/locations/global/batchPredictionJobs/7737084104264384512",
  "displayName": "my-cloud-storage-batch-inference-job",
  "model": "publishers/google/models/gemini-3-flash-preview",
  "inputConfig": {
    "instancesFormat": "jsonl",
    "gcsSource": {
      "uris": [
        "gs://gvideo.modelverse.cn/batch_prompt_for_batch_gemini_predict.jsonl"
      ]
    }
  },
  "outputConfig": {
    "predictionsFormat": "jsonl",
    "gcsDestination": {
      "outputUriPrefix": "gs://gvideo.modelverse.cn/test"
    }
  },
  "outputInfo": {
    "gcsOutputDirectory": "gs://gvideo.modelverse.cn/test/prediction-model-2026-02-28T08:22:20.427871Z"
  },
  "state": "JOB_STATE_SUCCEEDED",
  "completionStats": {
    "successfulCount": "2"
  },
  "createTime": "2026-02-28T08:22:22.313780Z",
  "startTime": "2026-02-28T08:23:12.172401Z",
  "endTime": "2026-02-28T08:27:35.914509Z",
  "updateTime": "2026-02-28T08:27:35.914509Z",
  "encryptionSpec": {},
  "modelVersionId": "1"
}
```

---

## 3. 查询批量任务状态

**请求**

- **方法 / 路径**：`GET /v1beta/batchPredictionJobs/:job_id`
- **路径参数**：`job_id` 为创建任务后得到的任务 ID（即 `name` 中最后一段或接口返回的 id）。

**响应**

返回 Vertex 风格任务详情，主要字段包括：

```json
{
  "name": "projects/279887244472/locations/global/batchPredictionJobs/7737084104264384512",
  "displayName": "my-cloud-storage-batch-inference-job",
  "model": "publishers/google/models/gemini-3-flash-preview",
  "inputConfig": {
    "instancesFormat": "jsonl",
    "gcsSource": {
      "uris": [
        "gs://gvideo.modelverse.cn/batch_prompt_for_batch_gemini_predict.jsonl"
      ]
    }
  },
  "outputConfig": {
    "predictionsFormat": "jsonl",
    "gcsDestination": {
      "outputUriPrefix": "gs://gvideo.modelverse.cn/test"
    }
  },
  "outputInfo": {
    "gcsOutputDirectory": "gs://gvideo.modelverse.cn/test/prediction-model-2026-02-28T08:22:20.4172871Z"
  },
  "state": "JOB_STATE_SUCCEEDED",
  "completionStats": {
    "successfulCount": "2"
  },
  "createTime": "2026-02-28T08:22:22.313780Z",
  "startTime": "2026-02-28T08:23:12.172401Z",
  "endTime": "2026-02-28T08:27:35.914509Z",
  "updateTime": "2026-02-28T08:27:35.914509Z",
  "encryptionSpec": {},
  "modelVersionId": "1"
}
```


| 字段                            | 类型     | 说明                             |
| ----------------------------- | ------ | ------------------------------ |
| name                          | string | 任务资源名或短 id                     |
| state                         | string | 任务状态，见下方状态码说明               |
| outputInfo.gcsOutputDirectory | string | 输出目录（用于下载时作为 file_id）          |
| completionStats               | object | 完成统计（如 successfulCount）        |

**任务状态码（state）**

| state 取值 | 含义 |
|------------|------|
| `JOB_STATE_PENDING` | 已创建，等待调度 |
| `JOB_STATE_RUNNING` | 执行中 |
| `JOB_STATE_SUCCEEDED` | 成功完成 |
| `JOB_STATE_FAILED` | 失败 |
| `JOB_STATE_CANCELLED` | 已取消 |
| `JOB_STATE_PAUSED` | 已暂停 |

任务完成后，使用 **outputInfo.gcsOutputDirectory** 作为下一步下载接口的 `file_id`（传入的是目录，不是单文件路径）。

**示例（Gemini 格式认证）**

```bash
curl -X GET "https://api.modelverse.cn/v1beta/batchPredictionJobs/<job_id>" \
  -H "x-goog-api-key: $MODELVERSE_API_KEY"
```

---

## 4. 下载批量任务结果文件

任务状态为完成后，可通过本接口下载该任务的结果文件（固定为 `predictions.jsonl`）。

**请求**

- **方法 / 路径**：`GET /v1/batchPredictionJobs/content?file_id=<gcsOutputDirectory>`
- **查询参数**：
  - **file_id**（必填）：任务**输出目录**的 GCS URI（即查询任务接口返回的 `outputInfo.gcsOutputDirectory`），例如 `gs://gemini-batch-001/output/prediction-model-2026-03-11T06:15:44.636340Z`。服务会在该目录下定位并返回 `predictions.jsonl`，因此传入的是**目录**，不要传单文件名。

**响应**

- 成功：返回 `predictions.jsonl` 文件流（如 `Content-Type: application/octet-stream` 或 `application/jsonl`）。
- 失败：返回相应 HTTP 状态码与错误信息。

**示例**

```bash
# 将 <output_directory> 替换为查询任务接口返回的 outputInfo.gcsOutputDirectory
curl -X GET "https://api.modelverse.cn/v1/batchPredictionJobs/content?file_id=<output_directory>" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -o predictions.jsonl
```

---

## 小结


| 步骤  | 接口                                                 | 说明                                                  |
| --- | -------------------------------------------------- | --------------------------------------------------- |
| 1   | `POST /v1/files`                                   | 上传输入文件，**purpose 固定为 batch:gcs**；返回的 id 用于拼成 gs:/// |
| 2   | `POST /v1beta/batchPredictionJobs`                 | 创建任务，**uris 为字符串**，填完整 GCS URI                      |
| 3   | `GET /v1beta/batchPredictionJobs/:job_id`          | 查询状态，从响应中取 **outputInfo.gcsOutputDirectory**        |
| 4   | `GET /v1/batchPredictionJobs/content?file_id=<目录>` | 下载结果，**file_id 填输出目录**，服务返回该目录下 predictions.jsonl   |


