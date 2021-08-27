1. First encrypt your email, user and password using `ansible-vault` [see wiki link](https://github.com/coopdevs/handbook/wiki/Ansible-vault). In example, the resulting vars can be named as this:
```
smtp_account_gmail_from: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          63386164316438393431636639343832663962663230353730623134353236313862633864376336
          ... numbers ...
          32303039336335363937363565336163613435333231356330316132343563646637

msmtp_account_gmail_user: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31386532366331643763353133623534653265643435356133343832623563303162646363633639
          ... numbers ...
          37306530316333333838616337323262396332316631643265303732303133316263

msmtp_account_gmail_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          66303933353539323464306133366663313266323132663762386239353163393064653734623065
          ... numbers ...
          3336
```

2. Add these vars to your inventory:
```
msmtp_default_account: "gmail"                                                                       
msmtp_alias_default : your.email@your.domain                                                       
msmtp_alias_root : your.email@your.domain                                                          
                                                                                                     
msmtp_accounts:                                                                                      
  - account: gmail                                                                                   
    host: smtp.gmail.com                                                                             
    port: 587                                                                                        
    auth: "on"                                                                                       
    from: "{{ msmtp_account_gmail_from }}"                                                           
    user: "{{ msmtp_account_gmail_user }}"                                                           
    password: "{{ msmtp_account_gmail_password }}"                                                   

```
3. Add this role to your `requirements.yml`
```
- src: chriswayg.msmtp-mailer
```
and this other line to your playbook:
```
    - role: chriswayg.msmtp-mailer
      tags: mail
```
(at the moment it has not releases, so no version line is needed)

4. Provision 
5. Activate `Less secured apps` in [Gmail](https://support.google.com/a/answer/6260879?hl=en)
6. Log into the provisioned machine and send you a test email (it will fail) using:
```
mail -s test your.email@your.domain
```
The output will be something like this:
```
send-mail: authentication failed (method PLAIN)
send-mail: server message: 534-5.7.14 <https://accounts.google.com/signin/continue?sarp=1&scc=1&plt=AKgnsbu
send-mail: server message: 534-5.7.14 y-qCoX8DmSUHvX3iXgNWq3EOGEgP_tMMVRft9N8DmZxZadY0WWbiI53PRc6gnP1fEKQCW
send-mail: server message: 534-5.7.14 wp46_xiVgK_GAYsx7yQM9EfEoWH7AluHR6YjOAOZJ8RtIIzgEq-Agtw68gbEWVd7>
send-mail: server message: 534-5.7.14 Please log in via your web browser and then try again.
send-mail: server message: 534-5.7.14  Learn more at
send-mail: server message: 534 5.7.14  https://support.google.com/mail/answer/78754 z19sm5633318wrg.28 - gsmtp
send-mail: could not send mail (account default from /etc/msmtprc)
Can't send mail: sendmail process failed with error code 77
```
7. Install `links` in your provisioned machine and open link that has been shown in the last step:
```
links "https://accounts.google.com/signin/continue?sarp=1&scc=1&plt=AKgnsbuy-qCoX8DmSUHvX3iXgNWq3EOGEgP_tMMVRft9N8DmZxZadY0WWbiI53PRc6gnP1fEKQCWwp46_xiVgK_GAYsx7yQM9EfEoWH7AluHR6YjOAOZJ8RtIIzgEq-Agtw68gbEWVd7"
```
8. Fulfill the authentication challenge from Gmail
9. Repeat the step 6. The email will be sent