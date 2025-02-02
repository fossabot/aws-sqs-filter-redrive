# aws-sqs-filter-redrive
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fnatenho%2Faws-sqs-filter-redrive.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fnatenho%2Faws-sqs-filter-redrive?ref=badge_shield)


`aws-sqs-filter-redrive` is a tool to selectively redrive Amazon Simple Queue Service (SQS) messages based on their content. With this tool, you can get and filter messages from a queue based on their attributes or JSON body fields, and then move or delete them to a new queue.

## Installation

```bash
go install github.com/natenho/aws-sqs-filter-redrive@latest
```

## Usage

```bash
aws-sqs-filter-redrive [OPTIONS]
```

Here are the available options:

- `-source`: The URL of the source queue.
- `-target`: The URL of the target queue.
- `-count`: The number of messages to delete or move.
- `-delete`: Delete the messages instead of moving them.
- `-move`: Move the messages to the target queue.
- `-dry`: Perform a dry run to show which messages would be processed.
- `-workers`: The number of workers to use (default: 8).
- `-polling-duration`: The duration for polling messages from the source queue (default: 30s).
- `-rate-limit`: The maximum number of messages to process per second (default: 10).
- `-before`: Get only messages sent before a certain date in format 2006-01-02T15:04:05Z07:00.
- `-after`: Get only messages sent after a certain date in format 2006-01-02T15:04:05Z07:00.
- `-attribute-filter`: Filter messages by a certain attribute using JQ expression.
- `-body-filter`: Filter messages by JSON body field content using JQ expression.

### Examples

#### Move messages that were sent after a date

```bash
aws-sqs-filter-redrive \
  -source https://sqs.us-east-1.amazonaws.com/123456789012/source-queue \
  -target https://sqs.us-east-1.amazonaws.com/123456789012/target-queue \
  -move \
  -after 2022-01-01T00:00:00Z
```

#### Delete messages sent before a date

```bash
aws-sqs-filter-redrive \
  -source https://sqs.us-east-1.amazonaws.com/123456789012/source-queue \
  -delete \
  -before 2022-01-01T00:00:00Z
```

#### Move messages from one queue to another based on a JSON body field

```bash
aws-sqs-filter-redrive \
  -source https://sqs.us-east-1.amazonaws.com/123456789012/source-queue \
  -target https://sqs.us-east-1.amazonaws.com/123456789012/target-queue \
  -move \
  -body-filter '.field == "value"'
```

#### Delete messages from a queue based on an attribute

```bash
aws-sqs-filter-redrive \
  -source https://sqs.us-east-1.amazonaws.com/123456789012/source-queue \
  -delete \
  -attribute-filter '.AttributeName == "AttributeValue"'
```

## Contributing

If you find a bug or have a feature request, please open an issue or submit a pull request. Contributions are always welcome!

## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fnatenho%2Faws-sqs-filter-redrive.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fnatenho%2Faws-sqs-filter-redrive?ref=badge_large)