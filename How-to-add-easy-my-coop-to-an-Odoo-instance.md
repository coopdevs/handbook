## Intro

_Easy my coop_ is a set of Odoo modules that help a cooperative to manage cooperator subscriptions and all the cooperative business processes. A very interesting module from this family is `easy_my_coop_website`, that uses the odoo `website` module to open a webpage where people can ask to join the consumer cooperative by providing data and buying a certain amount of shares.

## About Captcha dependency

The emc_website depends on `website_recaptcha_reloaded`, that imports via html's script, the google recaptcha reloaded, in order to protect the form from being abused. However, this comes also with a price.

### Benefits

Captcha's protect webform from being flooded with automated queries. Since Google bought recaptcha, they have monopolised the market. Their solution is widely used and effective against bots. Moreover, it's easier to use than other alternatives because it's centralised and one doesn't need and can't install their own captcha server module, although you need to obtain a key pair to identify yourself and your site against Google.

### Problems
In the other hand, by being centralised and requiring identifying keys, and being used everywhere, google captcha's  contribute heavily to the success of Surveillance Capitalism. At the same time, the rules become very agressive against dissident netizens, like tor users, javascript apostates and people blocking GAFAM content or just 3d party scripts.

### Two solutions

Until Coopdevs decides how to proceed, I have set up a captcha-free version of `easy_my_coop_website` module.

### With google captcha

When installing `easy_my_coop_website`, require version `12.0.1.0.0.99.dev23`. This has a dependency to relationship to `website_recaptcha_reloaded`. After the installation process, you will need to set up your Google site and public keys in "Settings" -> "Website Settings".

Until this is completed, the form won't work and will complain with a red text reading "the captcha has not been validated, please fill in the captcha", and a javascript error on the console reading "Error: Missing required parameters: sitekey".


**To do**: discover the amount of manual operations to manage these keys, and wether public key can be reused, and if site key can be automatically generated somehow.

### Without any captcha

When installing `easy_my_coop_website`, require version `12.0.1.0.0.99.dev28`. This one does not use any kind of captcha to slow down bots.

The module already validates server-side the format of the input data, and the data itself is not posted as comments anywhere, thus removing the incentive for trying to post viagra ads. Nevertheless, an open form is something dangerous and should be avoided.

## Installing "easy my coop"

Captcha apart, the changes to install emc are minimal.

Add to **files/requirements.txt**
```
git+https://gitlab.com/coopdevs/dummy-odoo-pypi-package@12.0#egg=odoo
odoo12-addon-easy-my-coop==12.0.3.0.1.99.dev40
# Uncomment the line below for captcha-free module
# odoo12-addon-easy-my-coop-website==12.0.1.0.0.99.dev28
# Uncomment the line below for captcha protected module
# odoo12-addon-easy-my-coop-website==12.0.1.0.0.99.dev23
```

Then, to the ansible variable `odoo_role_odoo_community_modules`, insert with alphabetic order `easy_my_coop,easy_my_coop_website`, or append it to the end.

Commit changes and provision to your test instance.

Once deployed, if you are using the captcha version, get the captcha keys and set them in Odoo.