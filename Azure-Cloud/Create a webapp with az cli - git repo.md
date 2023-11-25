
## Create an app plan with the following command
```bash
az appservice plan create -n appservice-plan-name -g resource-group-name --sku F1
```
>SKUs:
>
>B1, B2, B3, D1, F1, FREE, I1, I1v2, I2, I2v2, I3, I3v2, I4v2, I5v2, I6v2, P0V3, P1MV3, P1V2, P1V3, P2MV3, P2V2, P2V3, P3MV3, P3V2, P3V3, P4MV3, P5MV3, S1, S2, S3, SHARED, WS1, WS2, WS3

```bash
az webapp create -n webapp-name -g resource-group-name --plan appservice-plan-name
```

## Deploy git repo to the webapp with the following command
```bash
az webapp deployment source config --name webapp-name --resource-group resource-group-name --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
```
## Done :>