# serverless-receipt-processor-aws
Serverless pipeline using AWS Textract, Lambda, DynamoDB, S3, and SES to extract receipt data and automate expense processing.

--Serverless Receipt Processing System (AWS Textract + Lambda + DynamoDB)--

This project is a fully serverless pipeline that automatically extracts receipt data using AWS Textract, stores structured information in DynamoDB, and sends notifications via SES.

> Features

- Upload receipt to S3 → triggers processing
- Extract vendor, date, total, items using Textract AnalyzeExpense
- Stores structured data in DynamoDB
- Sends email notifications via SES
- Error-handling & logging in CloudWatch
- Fully serverless, scalable architecture
  

> Architecture

S3 (Upload Receipts)
        ↓ Trigger
AWS Lambda (Receipt Processor)
        ↓
AWS Textract (AnalyzeExpense API)
        ↓
DynamoDB (Structured Receipt Data)
        ↓
SES (Email Notification)


> Technologies Used

-AWS S3
-AWS Lambda (Python)
-AWS Textract
-AWS DynamoDB
-AWS SES
-CloudWatch Logs
-IAM


--Step-by-Step: How I Built the Serverless Receipt Processing System (AWS)--

1️⃣ Created AWS Resources
✓ S3 Bucket

Created an S3 bucket to upload receipt images/PDFs.
Enabled event notifications so every upload triggers a Lambda function.

2️⃣ Built the Lambda Function
✓ Language: Python
✓ Purpose: Process the receipt, extract details, store results, send email.

Steps inside Lambda:

A. Read S3 event
Lambda receives the bucket name + file key whenever a new receipt is uploaded.

B. Fetch the receipt from S3
Used boto3.client('s3').head_object() to verify the object exists.

3️⃣ Integrated AWS Textract (AnalyzeExpense API)
✓ Purpose: Extract structured data from the receipt

Lambda called:
textract.analyze_expense()

Textract automatically extracted:
Vendor
Date
Total amount
Line items (item name, price, quantity)

4️⃣ Stored Extracted Data in DynamoDB
✓ DynamoDB Table: Receipts

Lambda stored a structured JSON record:
receipt_id
vendor
date
total
items[]
s3_path
processed_timestamp

I made a mistake, checked and  fixed:
Table name mismatch
Region alignment
Environment variable override
Debugging via CloudWatch

After the correction → DynamoDB started storing data successfully.

5️⃣ Added Email Notifications with SES
✓ Action: After storing the receipt, SES sends an email containing:

Vendor
Total
Detected items list
Receipt ID
S3 location
This gives real-time confirmation that the receipt was processed.

6️⃣ CloudWatch Debugging & Monitoring

You used CloudWatch to debug:
DynamoDB table name mismatch
Textract output
Processing errors

Added:

print(f"DEBUG — Lambda is using DynamoDB table: {DYNAMODB_TABLE}")


This helped confirm the exact runtime behavior.

7️⃣ End-to-End Flow Working Successfully 🎉

Final pipeline works as:

Upload receipt → S3
      ↓ Trigger
Lambda → Textract → DynamoDB → SES Email


Everything is automated — zero manual steps.
