# Management REST API

## Authentication
- AWS signatureV4
- We use "minio" as region. Here region is set only for signature calculation.

## List of management APIs
- Service
  - Restart
  - Status
  - SetCredentials

- Locks
  - List
  - Clear

- Healing

### Service Management APIs
* Restart
  - POST /?service
  - x-minio-operation: restart
  - Response: On success 200

* Status
  - GET /?service
  - x-minio-operation: status
  - Response: On success 200, return json formatted object which contains StorageInfo and ServerVersion structures

* SetCredentials
  - GET /?service
  - x-minio-operation: set-credentials
  - Response: Success 200
  - Possible error responses
    - ErrMethodNotAllowed
    <Error>
        <Code>MethodNotAllowed</Code>
        <Message>The specified method is not allowed against this resource.</Message>
        <Key></Key>
        <BucketName></BucketName>
        <Resource>/</Resource>
        <RequestId>3L137</RequestId>
        <HostId>3L137</HostId>
    </Error>
    - ErrAdminBadCred
    <Error>
        <Code>XMinioBadCred</Code>
        <Message>XMinioBadCred</Message>
        <Key></Key>
        <BucketName></BucketName>
        <Resource>/</Resource>
        <RequestId>3L137</RequestId>
        <HostId>3L137</HostId>
    </Error>
    - ErrInternalError
    <Error>
        <Code>InternalError</Code>
        <Message>We encountered an internal error, please try again.</Message>
        <Key></Key>
        <BucketName></BucketName>
        <Resource>/</Resource>
        <RequestId>3L137</RequestId>
        <HostId>3L137</HostId>
    </Error>


### Lock Management APIs
* ListLocks
  - GET /?lock&bucket=mybucket&prefix=myprefix&duration=duration
  - x-minio-operation: list
  - Response: On success 200, json encoded response containing all locks held, for longer than duration.
  - Possible error responses
    - ErrInvalidBucketName
    <Error>
        <Code>InvalidBucketName</Code>
        <Message>The specified bucket is not valid.</Message>
        <Key></Key>
        <BucketName></BucketName>
        <Resource>/</Resource>
        <RequestId>3L137</RequestId>
        <HostId>3L137</HostId>
    </Error>

    - ErrInvalidObjectName
    <Error>
        <Code>XMinioInvalidObjectName</Code>
        <Message>Object name contains unsupported characters. Unsupported characters are `^*|\&#34;</Message>
        <Key></Key>
        <BucketName></BucketName>
        <Resource>/</Resource>
        <RequestId>3L137</RequestId>
        <HostId>3L137</HostId>
    </Error>

    - ErrInvalidDuration
      <Error>
          <Code>InvalidDuration</Code>
          <Message>Duration provided in the request is invalid.</Message>
          <Key></Key>
          <BucketName></BucketName>
          <Resource>/</Resource>
          <RequestId>3L137</RequestId>
          <HostId>3L137</HostId>
      </Error>


* ClearLocks
  - POST /?lock&bucket=mybucket&prefix=myprefix&duration=duration
  - x-minio-operation: clear
  - Response: On success 200, json encoded response containing all locks cleared, for longer than duration.
  - Possible error responses, similar to errors listed in ListLocks.
    - ErrInvalidBucketName
    - ErrInvalidObjectName
    - ErrInvalidDuration

### Healing

* ListBucketsHeal
  - GET /?heal
  - x-minio-operation: list-buckets
