# lamda
import boto3
import base64

s3 = boto3.client('s3')

def lambda_handler(event, context):
    try:
       
        file_content = event['body']
        file_name = event['file_name']
        bucket_name = 'resumedocumentbucket'
        padding = '=' * (4 - (len(file_content) % 4))
        padded_content = file_content + padding
        decoded_content = base64.b64decode(padded_content)
        pdf_file_path = file_name.replace('.docx', '.pdf')

        s3.put_object(Body=decoded_content, Bucket=bucket_name , Key=pdf_file_path)
        
        return {
            'statusCode': 200,
            'body': 'Success',
        }
    except Exception as e:
        return {
            'statusCode': 500,
            'body': e,
        }
