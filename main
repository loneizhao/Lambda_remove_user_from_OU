# This Lambda function receives notification from SNS and check the account id inside the notification, find the linked account id(second found ID) and then remove it from organizations, delete the budget plan and send notification

from __future__ import print_function
import json
import re
import boto3
sns = boto3.client('sns')
budgets = boto3.client('budgets')
topic = 'arn:aws:sns:us-west-2:904558036884:budget-action' # Repace SNS with your SNS
organizations = boto3.client('organizations')
print('Loading function')

def lambda_handler(event, context):
    message = event['Records'][0]['Sns']['Message']
    accountId= re.findall('\d{12}', message)
    print(message)
    print(accountId)
    if accountId:
        responseorg = organizations.remove_account_from_organization(
            AccountId= accountId[1]
              )

        response = budgets.delete_budget(
             AccountId=accountId[0],
             BudgetName=accountId[1]
             ) 
        snsmessage=accountId[1]+" has been moved out of organizations" 
        response = sns.publish(
         TopicArn=topic,
         Message=snsmessage,
         Subject='accountId responseorg',
)
    else: print('this is not a budget alert')

