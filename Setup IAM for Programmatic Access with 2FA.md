# Setup IAM for Programmatic Access with 2FA



## Create User in AWS Console



1. Create a role `u-role-prog-access-developer`

   * Set `Trust Entities` to current account
   * Enable 2FA: set `aws:MultiFactorAuthPresent` to `true` 

   ![image-20220110174243124](https://raw.githubusercontent.com/qinjie/picgo-images/main/image-20220110174243124.png)

   * Add minimal access rights

   ![image-20220110174541640](https://raw.githubusercontent.com/qinjie/picgo-images/main/image-20220110174541640.png)

2. Create a policy `u-iam-policy-prog-access-developer` to resume above role `u-role-prog-access-developer`.

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "sts:AssumeRole",
               "Resource": "arn:aws:iam::xxxxxxx:role/u-role-prog-access-developer",
               "Condition": {
                   "BoolIfExists": {
                       "aws:MultiFactorAuthPresent": "true"
                   }
               }
           }
       ]
   }
   ```

3. Create a user group `developers` and attach above policy `u-iam-policy-prog-access-developer` to this user group.

4. Create user, e.g. `qinjie-capdev`, and add user to above group.

## Configure CLI Profile

1. Setup profile a profile `capdev` (or any other name) using `aws configure`.

2. Edit `~/.aws/credential` file by 

   ```
   [capdev]
   aws_access_key_id = AKIAUOFXIF3X76CNP3B2
   aws_secret_access_key = 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
   source_profile = capdev
   mfa_serial = arn:aws:iam::xxxxx:mfa/qinjie-capdev
   role_arn = arn:aws:iam::xxxxx:role/u-role-prog-access-developer
   ```

   

