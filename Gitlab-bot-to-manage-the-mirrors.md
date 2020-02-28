# Mirroring repos to Gitlab

## Start/Update a pull mirror

1. Create a new project
2. Go to Settings > Repository > Mirroring repositories
3. Set the target repo URL
4. Set `Pull` as mirror direction
5. Set the authentication method
6. Set the usert to do the pull and push
7. Confirm the mirror rule

> You only can have one rule of pull mirror. Remove the old rules if you need to create new one with pull direction.

### OCB mirror - https://gitlab.com/coopdevs/OCB

We manage a mirror of Odoo [OCB](https://github.com/OCA/OCB.git) to publish releases. We need this releases but the main repository don't work with it.
