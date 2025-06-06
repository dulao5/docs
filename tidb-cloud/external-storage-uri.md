---
title: URI Formats of External Storage Services
summary: Learn about the storage URI formats of external storage services, including Amazon S3, GCS, and Azure Blob Storage.
---

## URI Formats of External Storage Services

This document describes the URI formats of external storage services, including Amazon S3, GCS, and Azure Blob Storage.

The basic format of the URI is as follows:

```shell
[scheme]://[host]/[path]?[parameters]
```

## Amazon S3 URI format

- `scheme`: `s3`
- `host`: `bucket name`
- `parameters`:

    - `access-key`: Specifies the access key.
    - `secret-access-key`: Specifies the secret access key.
    - `session-token`: Specifies the temporary session token. TiDB supports this parameter starting from v7.6.0.
    - `use-accelerate-endpoint`: Specifies whether to use the accelerate endpoint on Amazon S3 (defaults to `false`).
    - `endpoint`: Specifies the URL of custom endpoint for S3-compatible services (for example, `<https://s3.example.com/>`).
    - `force-path-style`: Use path style access rather than virtual hosted style access (defaults to `true`).
    - `storage-class`: Specifies the storage class of the uploaded objects (for example, `STANDARD` or `STANDARD_IA`).
    - `sse`: Specifies the server-side encryption algorithm used to encrypt the uploaded objects (value options: empty, `AES256`, or `aws:kms`).
    - `sse-kms-key-id`: Specifies the KMS ID if `sse` is set to `aws:kms`.
    - `acl`: Specifies the canned ACL of the uploaded objects (for example, `private` or `authenticated-read`).
    - `role-arn`: When you need to access Amazon S3 data from a third party using a specified [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html), you can specify the corresponding [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) of the IAM role with the `role-arn` URL query parameter, such as `arn:aws:iam::888888888888:role/my-role`. For more information about using an IAM role to access Amazon S3 data from TiDB Cloud, see [Configure External Storage Access for TiDB Cloud Dedicated](/tidbcloud/config-s3-and-gcs-access.md). TiDB supports this parameter starting from v7.6.0.
    - `external-id`: When you access Amazon S3 data from TiDB Cloud, you must need to specify a correct [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) to assume [the IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html). In this case, you must use this `external-id` URL query parameter to specify the external ID and make sure that you can assume the IAM role.

> **Note:**
> When configuring the IAM role, make sure to explicitly specify the trusted AWS account ID in the Principal field, and always include a unique external-id condition to prevent unauthorized access via [confused deputy attacks](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html).
> You can find the TiDB Cloud AWS account ID in TiDB Cloud, and use AWS CloudFormation to create the IAM role securely in one click by following the linked documentation, see [Configure External Storage Access for TiDB Cloud Dedicated](/tidbcloud/config-s3-and-gcs-access.md).
> Optionally, you may also set a max-session-duration to limit the lifetime of temporary credentials for enhanced security.
> 

The following is an example of an Amazon S3 URI for [`BACKUP`](/tidbcloud/sql-statement-backup.md) and [`RESTORE`](/tidbcloud/sql-statement-restore.md). In this example, you need to specify a specific file path `testfolder`.

```shell
s3://external/testfolder?access-key=${access-key}&secret-access-key=${secret-access-key}
```

The following is an example of an Amazon S3 URI for [`IMPORT INTO`](/tidbcloud/sql-statement-import-into.md). In this example, you need to specify a specific filename `test.csv`.

```shell
s3://external/test.csv?access-key=${access-key}&secret-access-key=${secret-access-key}
```

## GCS URI format

- `scheme`: `gcs` or `gs`
- `host`: `bucket name`
- `parameters`:

    - `credentials-file`: Specifies the path to the credentials JSON file on the migration tool node.
    - `storage-class`: Specifies the storage class of the uploaded objects (for example, `STANDARD` or `COLDLINE`)
    - `predefined-acl`: Specifies the predefined ACL of the uploaded objects (for example, `private` or `project-private`)

The following is an example of a GCS URI for TiDB Lightning and BR. In this example, you need to specify a specific file path `testfolder`.

```shell
gcs://external/testfolder?credentials-file=${credentials-file-path}
```

The following is an example of a GCS URI for [`IMPORT INTO`](/tidbcloud/sql-statement-import-into.md). In this example, you need to specify a specific filename `test.csv`.

```shell
gcs://external/test.csv?credentials-file=${credentials-file-path}
```

## Azure Blob Storage URI format

- `scheme`: `azure` or `azblob`
- `host`: `container name`
- `parameters`:

    - `account-name`: Specifies the account name of the storage.
    - `account-key`: Specifies the access key.
    - `sas-token`: Specifies the shared access signature (SAS) token.
    - `access-tier`: Specifies the access tier of the uploaded objects, for example, `Hot`, `Cool`, or `Archive`. The default value is the default access tier of the storage account.
    - `encryption-scope`: Specifies the [encryption scope](https://learn.microsoft.com/en-us/azure/storage/blobs/encryption-scope-manage?tabs=powershell#upload-a-blob-with-an-encryption-scope) for server-side encryption.
    - `encryption-key`: Specifies the [encryption key](https://learn.microsoft.com/en-us/azure/storage/blobs/encryption-customer-provided-keys) for server-side encryption, which uses the AES256 encryption algorithm.

The following is an example of an Azure Blob Storage URI for BR. In this example, you need to specify a specific file path `testfolder`.

```shell
azure://external/testfolder?account-name=${account-name}&account-key=${account-key}
```
