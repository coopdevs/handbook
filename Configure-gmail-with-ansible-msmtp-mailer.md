First of all crypt your email, user and password using `ansible-vault` [see wiki link](https://github.com/coopdevs/handbook/wiki/Ansible-vault). In example, the resulting vars can be named as this:
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