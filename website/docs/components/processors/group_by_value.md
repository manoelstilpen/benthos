---
title: group_by_value
type: processor
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/processor/group_by_value.go
-->


Splits a batch of messages into N batches, where each resulting batch contains a
group of messages determined by a
[function interpolated string](/docs/configuration/interpolation#functions) evaluated
per message.

```yaml
group_by_value:
  value: ${!metadata:example}
```

This allows you to group messages using arbitrary fields within their content or
metadata, process them individually, and send them to unique locations as per
their group.

For example, if we were consuming Kafka messages and needed to group them by
their key, archive the groups, and send them to S3 with the key as part of the
path we could achieve that with the following:

``` yaml
pipeline:
  processors:
  - group_by_value:
      value: ${!metadata:kafka_key}
  - archive:
      format: tar
  - compress:
      algorithm: gzip
output:
  s3:
    bucket: TODO
    path: docs/${!metadata:kafka_key}/${!count:files}-${!timestamp_unix_nano}.tar.gz
```

The functionality of this processor depends on being applied across messages
that are batched. You can find out more about batching [in this doc](/docs/configuration/batching).

## Fields

### `value`

`string` The interpolated string to group based on.

This field supports [interpolation functions](/docs/configuration/interpolation#functions).

```yaml
# Examples

value: ${!metadata:kafka_key}

value: ${!json_field:foo.bar}-${!metadata:baz}
```

