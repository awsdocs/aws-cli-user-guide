# Amazon S3 bucket lifecycle operations scripting example<a name="cli-services-s3-lifecycle-example"></a>

This topic uses a bash scripting example for Amazon S3 bucket lifecycle operations using the AWS Command Line Interface \(AWS CLI\)\. This scripting example uses the [https://docs.aws.amazon.com/cli/latest/reference/s3api/](https://docs.aws.amazon.com/cli/latest/reference/s3api/) set of commands\. Shell scripts are programs designed to run in a command line interface\.

**Topics**
+ [Before you start](#cli-services-s3-lifecycle-example-before)
+ [About this example](#cli-services-s3-lifecycle-example-about)
+ [Files](#cli-services-s3-lifecycle-example-files)
+ [References](#cli-services-s3-lifecycle-example-references)

## Before you start<a name="cli-services-s3-lifecycle-example-before"></a>

Before you can run any of the below examples, the following things need to be completed\.
+ AWS CLI installed, see [Installing, updating, and uninstalling the AWS CLI](cli-chap-install.md) for more information\.
+ AWS CLI configured, see [Configuration basics](cli-configure-quickstart.md) for more information\. The profile that you use must have permissions that allow the AWS operations performed by the examples\.
+ As an AWS best practice, grant this code least privilege, or only the permissions required to perform a task\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ This code has not been tested in all AWS Regions\. Some AWS services are available only in specific Regions\. For more information, see [ Service Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html) in the *AWS General Reference Guide*\. 
+ Running this code can result in charges to your AWS account\. It is your responsibility to ensure that any resources created by this script are removed when you are done with them\. 

The Amazon S3 service uses the following terms:
+ Bucket — A top level Amazon S3 folder\.
+ Prefix — An Amazon S3 folder in a bucket\.
+ Object — Any item hosted in an Amazon S3 bucket\.

## About this example<a name="cli-services-s3-lifecycle-example-about"></a>

This example demonstrates how to interact with some of the basic Amazon S3 operations using a set of functions in shell script files\. The functions are located in the shell script file named `bucket-operations.sh`\. You can call these functions in another file\. Each script file contains comments describing each of the functions\.

To see the intermediate results of each step, run the script with a `-i` parameter\. You can view the current status of the bucket or its contents using the Amazon S3 console\. The script only proceeds to the next step when you press **enter** at the prompt\. 

For the full example and downloadable script files, see [Amazon S3 Bucket Lifecycle Operations](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/aws-cli/bash-linux/s3/bucket-lifecycle-operations) in the *AWS Code Examples Repository* on *GitHub*\.

## Files<a name="cli-services-s3-lifecycle-example-files"></a>

The example contains the following files:

**bucket\-operations\.sh**  
This main script file can be sourced from another file\. It includes functions that perform the following tasks:  
+ Creating a bucket and verifying that it exists
+ Copying a file from the local computer to a bucket
+ Copying a file from one bucket location to a different bucket location
+ Listing the contents of a bucket
+ Deleting a file from a bucket
+ Deleting a bucket

### Code<a name="cli-services-s3-lifecycle-example-bucket-operations"></a>

```
source ./awsdocs_general.sh

###############################################################################
# function bucket_exists
#
# This function checks to see if the specified bucket already exists.
#
# Parameters:
#       $1 - The name of the bucket to check
# 
# Returns:
#       0 if the bucket already exists
#       1 if the bucket doesn't exist
###############################################################################
function bucket_exists {
    be_bucketname=$1

    # Check whether the bucket already exists. 
    # We suppress all output - we're interested only in the return code.

    aws s3api head-bucket \
        --bucket $be_bucketname \
        >/dev/null 2>&1

    if [[ ${?} -eq 0 ]]; then
        return 0        # 0 in Bash script means true.
    else
        return 1        # 1 in Bash script means false.
    fi
}
###############################################################################
# function create-bucket
#
# This function creates the specified bucket in the specified AWS Region, unless
# it already exists.
# 
# Parameters:
#       -b bucket_name  -- The name of the bucket to create
#       -r region_code  -- The code for an AWS Region in which to 
#                          create the bucket
# 
# Returns:
#       The URL of the bucket that was created.
#     And:
#       0 if successful
#       1 if it fails
###############################################################################
function create_bucket {
    local BUCKET_NAME REGION_CODE RESPONSE
    local OPTION OPTIND OPTARG # Required to use getopts command in a function 

    function usage {
        echo "function create_bucket"
        echo "Creates an Amazon S3 bucket. You must supply both of the following parameters:"
        echo "  -b bucket_name    The name of the bucket. It must be globally unique."
        echo "  -r region_code    The code for an AWS Region in which the bucket is created."
        echo ""
    }

    # Retrieve the calling parameters
    while getopts "b:r:" OPTION; do
        case "${OPTION}"
        in
            b)  BUCKET_NAME="${OPTARG}";;
            r)  REGION_CODE="${OPTARG}";;
            h)  usage; return 0;;
            \?) echo "Invalid parameter"; usage; return 1;; 
        esac
    done

    if [[ -z "$BUCKET_NAME" ]]; then
        errecho "ERROR: You must provide a bucket name with the -b parameter."
        usage
        return 1
    fi

    if [[ -z "$REGION_CODE" ]]; then
        errecho "ERROR: You must provide an AWS Region code with the -r parameter."
        usage
        return 1
    fi

    iecho "Parameters:\n"
    iecho "    Bucket name:   $BUCKET_NAME"
    iecho "    Region code:   $REGION_CODE"
    iecho ""
    
    
    # If the bucket already exists, we don't want to try to create it.
    if (bucket_exists $BUCKET_NAME); then 
        errecho "ERROR: A bucket with that name already exists. Try again."
        return 1
    fi

    # The bucket doesn't exist, so try to create it.
    
    RESPONSE=$(aws s3api create-bucket \
                --bucket $BUCKET_NAME \
                --create-bucket-configuration LocationConstraint=$REGION_CODE)

    if [[ ${?} -ne 0 ]]; then
        errecho "ERROR: AWS reports create-bucket operation failed.\n$RESPONSE"
        return 1
    fi
}

###############################################################################
# function copy_file_to_bucket
#
# This function creates a file in the specified bucket. 
#
# Parameters:
#       -b bucket_name$1 - The name of the bucket to copy the file to
#       $2 - The path and file name of the local file to copy to the bucket
#       $3 - The key (name) to call the copy of the file in the bucket
# 
# Returns:
#       0 if successful
#       1 if it fails
###############################################################################
function copy_file_to_bucket {
    cftb_bucketname=$1
    cftb_sourcefile=$2
    cftb_destfilename=$3
    local RESPONSE
    
    RESPONSE=$(aws s3api put-object \
                --bucket $cftb_bucketname \
                --body $cftb_sourcefile \
                --key $cftb_destfilename)

    if [[ ${?} -ne 0 ]]; then
        errecho "ERROR: AWS reports put-object operation failed.\n$RESPONSE"
        return 1
    fi
}

###############################################################################
# function copy_item_in_bucket
#
# This function creates a copy of the specified file in the same bucket.
#
# Parameters:
#       $1 - The name of the bucket to copy the file from and to
#       $2 - The key of the source file to copy
#       $3 - The key of the destination file
# 
# Returns:
#       0 if successful
#       1 if it fails
###############################################################################
function copy_item_in_bucket {
    ciib_bucketname=$1
    ciib_sourcefile=$2
    ciib_destfile=$3
    local RESPONSE
    
    RESPONSE=$(aws s3api copy-object \
                --bucket $ciib_bucketname \
                --copy-source $ciib_bucketname/$ciib_sourcefile \
                --key $ciib_destfile)

    if [[ $? -ne 0 ]]; then
        errecho "ERROR:  AWS reports s3api copy-object operation failed.\n$RESPONSE"
        return 1
    fi
}

###############################################################################
# function list_items_in_bucket
#
# This function displays a list of the files in the bucket with each file's 
# size. The function uses the --query parameter to retrieve only the Key and 
# Size fields from the Contents collection.
#
# Parameters:
#       $1 - The name of the bucket
# 
# Returns:
#       The list of files in text format
#     And:
#       0 if successful
#       1 if it fails
###############################################################################
function list_items_in_bucket {
    liib_bucketname=$1
    local RESPONSE

    RESPONSE=$(aws s3api list-objects \
                --bucket $liib_bucketname \
                --output text \
                --query 'Contents[].{Key: Key, Size: Size}' )

    if [[ ${?} -eq 0 ]]; then
        echo "$RESPONSE"
    else
        errecho "ERROR: AWS reports s3api list-objects operation failed.\n$RESPONSE"
        return 1
    fi
}

###############################################################################
# function delete_item_in_bucket
#
# This function deletes the specified file from the specified bucket. 
#
# Parameters:
#       $1 - The name of the bucket
#       $2 - The key (file name) in the bucket to delete

# Returns:
#       0 if successful
#       1 if it fails
###############################################################################
function delete_item_in_bucket {
    diib_bucketname=$1
    diib_key=$2
    local RESPONSE
    
    RESPONSE=$(aws s3api delete-object \
                --bucket $diib_bucketname \
                --key $diib_key)

    if [[ $? -ne 0 ]]; then
        errecho "ERROR:  AWS reports s3api delete-object operation failed.\n$RESPONSE"
        return 1
    fi
}

###############################################################################
# function delete_bucket
#
# This function deletes the specified bucket.
#
# Parameters:
#       $1 - The name of the bucket

# Returns:
#       0 if successful
#       1 if it fails
###############################################################################
 function delete_bucket {
    db_bucketname=$1
    local RESPONSE

    RESPONSE=$(aws s3api delete-bucket \
                --bucket $db_bucketname)

    if [[ $? -ne 0 ]]; then
        errecho "ERROR: AWS reports s3api delete-bucket failed.\n$RESPONSE"
        return 1
    fi
}
```

**test\-bucket\-operations\.sh**  
The shell script file `test-bucket-operations.sh` demonstrates how to call the functions by sourcing the `bucket-operations.sh` file and calling each of the functions\. After calling functions, the test script removes all resources that it created\. 

### Code<a name="cli-services-s3-lifecycle-example-bucket-operations-test"></a>

```
source ./awsdocs_general.sh
source ./bucket_operations.sh

function usage {
    echo "This script tests Amazon S3 bucket operations in the AWS CLI."
    echo "It creates a randomly named bucket, copies files to it, then"
    echo "deletes the files and the bucket."
    echo ""
    echo "To pause the script between steps so you can see the results in the"
    echo "AWS Management Console, include the parameter -i."
    echo ""
    echo "IMPORTANT: Running this script creates resources in your Amazon"
    echo "   account that can incur charges. It is your responsibility to"
    echo "   ensure that no resources are left in your account after the script"
    echo "   completes. If an error occurs during the operation of the script,"
    echo "   then resources can remain that you might need to delete manaully."
}

    # Set default values.
    INTERACTIVE=false
    VERBOSE=false

    # Retrieve the calling parameters
    while getopts "ivh" OPTION; do
        case "${OPTION}"
        in
            i)  INTERACTIVE=true;VERBOSE=true; iecho;;
            v)  VERBOSE=true;;
            h)  usage; return 0;;
            \?) echo "Invalid parameter"; usage; return 1;; 
        esac
    done


if [ "$INTERACTIVE" == "true" ]; then iecho "Tests running in interactive mode."; fi
if [ "$VERBOSE" == "true" ]; then iecho "Tests running in verbose mode."; fi

iecho "***************SETUP STEPS******************"
BUCKETNAME=$(generate_random_name s3test)
REGION="us-west-2"
FILENAME1=$(generate_random_name s3testfile)
FILENAME2=$(generate_random_name s3testfile)

iecho "BUCKETNAME=$BUCKETNAME"
iecho "REGION=$REGION"
iecho "FILENAME1=$FILENAME1"
iecho "FILENAME2=$FILENAME2"

iecho "**************END OF STEPS******************"

run_test "1. Creating bucket with missing bucket_name" \
         "create_bucket -r $REGION" \
         1 \
         "ERROR: You must provide a bucket name" \


run_test "2. Creating bucket with missing region_name" \
         "create_bucket -b $BUCKETNAME" \
         1 \
         "ERROR: You must provide an AWS Region code"

run_test "3. Creating bucket with valid parameters" \
         "create_bucket -r $REGION -b $BUCKETNAME" \
         0

run_test "4. Creating bucket with duplicate name and region" \
         "create_bucket -r $REGION -b $BUCKETNAME" \
         1 \
         "ERROR: A bucket with that name already exists"

run_test "5. Copying local file (copy of this script) to bucket" \
         "copy_file_to_bucket $BUCKETNAME ./$0 $FILENAME1" \
         0

run_test "6. Duplicating existing file in bucket" \
         "copy_item_in_bucket $BUCKETNAME $FILENAME1 $FILENAME2" \
         0

run_test "7. Listing contents of bucket" \
         "list_items_in_bucket $BUCKETNAME" \
         0

run_test "8. Deleting first file from bucket" \
         "delete_item_in_bucket $BUCKETNAME $FILENAME1" \
         0

run_test "9. Deleting second file from bucket" \
         "delete_item_in_bucket $BUCKETNAME $FILENAME2" \
         0

run_test "10. Deleting bucket" \
         "delete_bucket $BUCKETNAME" \
         0

echo "Tests completed successfully."
```

**awsdocs\-general\.sh**  
The script file `awsdocs-general.sh` holds general purpose functions used across advanced code examples for the AWS CLI\.

### Code<a name="cli-services-s3-lifecycle-example-general"></a>

```
# Set global defaults:
VERBOSE=false

###############################################################################
# function run_test
#
# This function is used to perform a command and compare its output to both
# the expected error code and the expected output string. If there isn't a 
# match, then the function invokes the test_failed function.
###############################################################################
function run_test {
    local DESCRIPTION COMMAND EXPECTED_ERR_CODE EXPECTED_OUTPUT RESPONSE
    
    DESCRIPTION="$1"
    COMMAND="$2"
    EXPECTED_ERR_CODE="$3"
    if [[ -z "$4" ]]; then EXPECTED_OUTPUT="$4"; else EXPECTED_OUTPUT=""; fi
    
    iecho -n "Running test: $DESCRIPTION..."
    RESPONSE="$($COMMAND)"
    ERR="${?}"

    # Check to see if we got the expected error code.
    if [[ "$EXPECTED_ERR_CODE" -ne "$ERR" ]]; then
        test_failed "The test \"$DESCRIPTION\" returned an unexpected error code: $ERR"
    fi

    #now check the error message, if we provided other than "".
    if [[ -n "$EXPECTED_OUTPUT" ]]; then
        MATCH=$(echo "$RESPONSE" | grep "$EXPECTED_OUTPUT")
        # If there was no match (it's an empty string), then fail.
        if [[ -z "$MATCH" ]]; then
            test_failed "The test \"$DESCRIPTION\" returned an unexpected output: $RESPONSE"
        fi
    fi
    
    iecho "OK"
    ipause
}

###############################################################################
# function test_failed
#
# This function is used to terminate a failed test and to warn the customer
# about possible undeleted resources that could incur costs to their account.
###############################################################################

function test_failed {
    
    errecho ""
    errecho "===TEST FAILED==="
    errecho "$@"
    errecho ""
    errecho "    One or more of the tests failed to complete successfully. This means that any"
    errecho "    tests after the one that failed test didn't run and might have left resources"
    errecho "    still active in your account."
    errecho ""
    errecho "IMPORTANT:"
    errecho "    Resources created by this script can incur charges to your AWS account. If the"
    errecho "    script did not complete successfully, then you must review and manually delete"
    errecho "    any resources created by this script that were not automatically removed."
    errecho ""
    exit 1 
}


###############################################################################
# function errecho
#
# This function outputs everything sent to it to STDERR (standard error output).
###############################################################################
function errecho {
    printf "%s\n" "$*" 2>&1
}

###############################################################################
# function iecho
#
# This function enables the script to display the specified text only if 
# the global variable $VERBOSE is set to true.
###############################################################################
function iecho {
    if [[ $VERBOSE == true ]]; then
        echo "$@"
    fi
}

###############################################################################
# function ipause
#
# This function enables the script to pause after each command if interactive
# mode is set (by including -i on the script invocation command).
###############################################################################
function ipause {
    if [[ $INTERACTIVE == true ]]; then
        read -r -p "Press ENTER to continue..."
    fi
}

# Initialize the shell's RANDOM variable
RANDOM=$$
###############################################################################
# function generate_random_name
#
# This function generates a random file name with using the specified root
# followed by 4 groups that each have 4 digits.
# The default root name is "test"
function generate_random_name {

    ROOTNAME="test"
    if [[ -n $1 ]]; then
        ROOTNAME=$1
    fi

    # Initialize the filename variable
    FILENAME="$ROOTNAME"
    # Configure random number generator to issue numbers between 1000 and 9999, inclusive
    DIFF=$((9999-1000+1))

    for _ in {1..4}
    do
        rnd=$(($((RANDOM%DIFF))+X))
        # make sure that the number is 4 digits long
        while [ "${#rnd}" -lt 4 ]; do rnd="0$rnd"; done
        FILENAME+="-$rnd"
    done
    echo $FILENAME
}
```

## References<a name="cli-services-s3-lifecycle-example-references"></a>

**AWS CLI reference:**
+ [aws s3api](https://docs.aws.amazon.com/cli/latest/reference/s3api/index.html)
+ [aws s3api create\-bucket](https://docs.aws.amazon.com/cli/latest/reference/s3api/create-bucket.html)
+ [aws s3api copy\-object](https://docs.aws.amazon.com/cli/latest/reference/s3api/copy-object.html)
+ [aws s3api delete\-bucket](https://docs.aws.amazon.com/cli/latest/reference/s3api/delete-bucket.html)
+ [aws s3api delete\-object](https://docs.aws.amazon.com/cli/latest/reference/s3api/delete-object.html)
+ [aws s3api head\-bucket](https://docs.aws.amazon.com/cli/latest/reference/s3api/head-bucket.html)
+ [aws s3api list\-objects](https://docs.aws.amazon.com/cli/latest/reference/s3api/list-objects.html)
+ [aws s3api put\-object](https://docs.aws.amazon.com/cli/latest/reference/s3api/put-object.html)

**Other reference:**
+ [Working with Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) in the *Amazon Simple Storage Service Developer Guide*
+ [Working with Amazon S3 objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingObjects.html) in the *Amazon Simple Storage Service Developer Guide*
+ To view and contribute to AWS SDK and AWS CLI code examples, see the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/) on *GitHub*\.