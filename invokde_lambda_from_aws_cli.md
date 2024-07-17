To invoke a Lambda function named `data-cleanup-lambda` from the AWS Command Line Interface (CLI), you can use the `aws lambda invoke` command. Here's a step-by-step guide:

1. **Open your Terminal or Command Prompt**: This is where you will enter the AWS CLI commands.

2. **Ensure AWS CLI is Installed and Configured**: Before you begin, make sure that you have the AWS CLI installed on your machine. If you haven't installed it yet, you can download it from the [AWS website](https://aws.amazon.com/cli/). Also, ensure that you have configured it with the necessary credentials and default region using the command `aws configure`.

3. **Invoke the Lambda Function**: To invoke your `data-cleanup-lambda` function, use the following command:

   ```bash
   aws lambda invoke --function-name data-cleanup-lambda output.txt
   ```

   Here, `data-cleanup-lambda` is the name of your Lambda function, and `output.txt` is the file where the response from the Lambda function will be saved. If you don't want to save the output to a file, you can direct it to `stdout` by using `-` as the output file:

   ```bash
   aws lambda invoke --function-name data-cleanup-lambda -
   ```

4. **Optional Parameters**: Depending on your Lambda function's configuration, you may need to pass additional parameters such as invocation type, function version, or payload. For example, if your function expects a JSON payload, you can provide it like this:

   ```bash
   aws lambda invoke --function-name data-cleanup-lambda --payload '{"key1":"value1", "key2":"value2"}' output.txt
   ```

5. **Check the Response**: After running the command, check `output.txt` (or your command line output if you directed it to `stdout`) for the response from your Lambda function. The CLI will also provide information about the invocation status.

Remember that your AWS IAM user must have the necessary permissions to invoke Lambda functions. If you encounter permission issues, you'll need to update your IAM policy accordingly.
