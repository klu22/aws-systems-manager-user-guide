# Creating Systems Manager parameters<a name="sysman-paramstore-su-create"></a>

Use the information in the following topics to help you create Systems Manager parameters using the AWS Systems Manager console, the AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell \(Tools for Windows PowerShell\)\.

This section demonstrates how to create, store, and run parameters with Parameter Store in a test environment\. It also demonstrates how to use Parameter Store with other Systems Manager capabilities and AWS services\. For more information, see [What is a parameter?](systems-manager-parameter-store.md#what-is-a-parameter)

## About requirements and constraints for parameter names<a name="sysman-parameter-name-constraints"></a>

Use the information in this topic to help you specify valid values for parameter names when you create a parameter\. 

This information supplements the details in the topic [PutParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutParameter.html) in the *AWS Systems Manager API Reference*, which also provides information about the values **AllowedPattern**, **Description**, **KeyId**, **Overwrite**, **Type**, and **Value**\.

The requirements and constraints for parameter names include the following:
+ **Case sensitivity**: Parameter names are case sensitive\.
+ **Spaces**: Parameter names can't include spaces\.
+ **Valid characters**: Parameter names can consist of the following symbols and letters only: `a-zA-Z0-9_.-`

  In addition, the slash character \( / \) is used to delineate hierarchies in parameter names\. For example: `/Dev/Production/East/Project-ABC/MyParameter`
+ **Valid AMI format**: When you choose `aws:ec2:image` as the data type for a `String` parameter, the ID you enter must validate for the AMI ID format `ami-12345abcdeEXAMPLE`\.
+ **Fully qualified**: When you create or reference a parameter in a hierarchy, include a leading forward slash character \(/\) \.
  + Fully qualified parameter names: `MyParameter1`, `/MyParameter2`, `/Dev/Production/East/Project-ABC/MyParameter`
  + Not fully qualified parameter name: `MyParameter3/L1`
+ **Length**: The maximum length for a parameter name that you create is 1011 characters\. This includes the characters in the ARN that precede the name you specify, such as `arn:aws:ssm:us-east-2:111122223333:parameter/`\.
+ **Prefixes**: A parameter name can't be prefixed with "`aws`" or "`ssm`" \(case\-insensitive\)\. For example, attempts to create parameters with the following names fail with an exception:
  + `awsTestParameter`
  + `SSM-testparameter`
  + `/aws/testparam1`
**Note**  
When you specify a parameter in an SSM document, command, or script, include `ssm` as part of the syntax\. For example, \{\{ssm:*parameter\-name*\}\} and \{\{ ssm:*parameter\-name* \}\}, such as `{{ssm:MyParameter}}`, and `{{ ssm:MyParameter }}.`
+ **Uniqueness**: A parameter name must be unique within an AWS Region\. For example, Systems Manager treats the following as separate parameters, if they exist in the same Region:
  + `/Test/TestParam1`
  + `/TestParam1`

  The following examples are also unique:
  + `/Test/TestParam1/Logpath1`
  + `/Test/TestParam1`

  The following examples, however, if in the same Region, aren't unique:
  + `/TestParam1`
  + `TestParam1`
+ **Hierarchy depth**: If you specify a parameter hierarchy, the hierarchy can have a maximum depth of fifteen levels\. You can define a parameter at any level of the hierarchy\. Both of the following examples are structurally valid:
  + `/Level-1/L2/L3/L4/L5/L6/L7/L8/L9/L10/L11/L12/L13/L14/parameter-name`
  + `parameter-name`

  Attempting to create the following parameter would fail with a `HierarchyLevelLimitExceededException` exception:
  + `/Level-1/L2/L3/L4/L5/L6/L7/L8/L9/L10/L11/L12/L13/L14/L15/L16/parameter-name`

**Important**  
If a user has access to a path, then the user can access all levels of that path\. For example, if a user has permission to access path `/a`, then the user can also access `/a/b`\. Even if a user has explicitly been denied access in AWS Identity and Access Management \(IAM\) for parameter `/a/b`, they can still call the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API operation recursively for `/a` and view `/a/b`\.

**Topics**
+ [About requirements and constraints for parameter names](#sysman-parameter-name-constraints)
+ [Create a Systems Manager parameter \(console\)](parameter-create-console.md)
+ [Create a Systems Manager parameter \(AWS CLI\)](param-create-cli.md)
+ [Create a Systems Manager parameter \(Tools for Windows PowerShell\)](param-create-ps.md)
