# Workflow Method Sample

Tile and connector to test kivapublic endpoints and the sending of realtime events

## Best practices

- We recommend using the [Use this template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template) feature to create a new repository.

- Cloning or forking is a satisfactory alternative.

- Any changes to the connector should be contained to your copy of the connector project only. You should never push any commits (and associated branches) to this repo.
## Connector Endpoints

| Connector Endpoint            | Kiva permissions required    |
| :---------------------------- | :--------------------------- |
| retrieveAccountList           | getAccounts                  |
| retrieveAccountList           | getAccountsRefresh           |
| retrieveTransactionList       | getTransactions              |
| retrieveTransactionCategories | getTransactionCategories     |
| editTransaction               | updateTransaction            |
| stopPayment                   | createStopPayment            |
| multiCall                     | getAccounts, getTransactions |
| retrieveUserBySocial          | getPartyBySSN                |
| retrieveUserById              | getPartyById                 |
| validateMemberAccountInfo     | validateMemberAccountInfo    |
| startTransfer                 | createInternalTransfer       |
| p2pTransfer                   | personToPersonTransfer       |
| subscribe                     | none                         |
| sendRealtimeEvent             | none                         |

## tileconfig.json

The following realtime events are currently supported in the platform -- you can add these to the tileconfig to allow them to be selected for subscribing to events or for sending events.

```json
{
  "eventNames": [
    "platform_account_transactionadded",
    "account_transactionadded",
    "account_balanceupdated",
    "account_nicknameupdated",
    "account_refreshall"
  ]
}
```

## How to test locally

When running the tile locally, uncomment the top section of the index.html file between the comments
`<!-- LOCAL DEVELOPMENT ONLY -->`

```html
<!--    <script src="https://assets.cdp.wiki/cdp_bundle.js?100"></script> -->
<!--    <link-->
<!--      rel="stylesheet"-->
<!--      href="https://assets.cdp.wiki/cdp_defaultTheme.css?100"-->
<!--      type="text/css"-->
<!--    />-->
<!--    <link rel="stylesheet" href="tile.css" />-->
<!--    <script src="tile.js"></script>-->
```

You cannot run the connector locally without a proxy because the endpoints require platform dependencies like kivapublic and redis.

## How to deploy

### Tile

Ensure that the script and link tags in the index.html file between `<!-- LOCAL DEVELOPMENT ONLY -->` are removed or commented out before deploying the tile
Upload the following files to the tile project in the portal:
tile.js
index.html
tileconfig.json
tilestrings-en.json
tile.less

### Connector
## Build script
```
run `python3 build-deploy.py <name of deployed connector> <version of deployed connector>`
```
## Local Structure example

```
basic_connector_sample/
├─ Dockerfile/
├─ lib/
├─ pom.xml/
├─ readme
├─ src/
├─ target/
```

## Portal Upload Structure

- The portal is only expecting the following required files at the time of upload ```externalconnector.zip``` ```pom.xml``` ```Dockerfile```

```
externalconnector.zip/ - Contains all the java code to build a project in a springboot self-contained application.
```
```
pom.xml/ - Includes all of the dependencies required to build the Java project.
```
```
Dockerfile/ - allows scripting to be performed in the container at deployment time. Only CDP can update this file in the portal
```

### externalconnector.zip structure:
```
src/: Java code that implements the third-party service
lib/: assets for monitoring the connector
├─ env.sh
├─ dd-java-agent.jar

For SOAP services:

- Supporting WSDL contract file and .xsd files
- A JAR file containing all the java files generated by the WSDL
```

## Upload Error Troubleshooting

- Make sure the name and version of your connector is reflected accurately in all of the connector assets uploaded to the portal

- BOTH lib/ and src/ directories should be packaged(compressed) together at the same level, in a ```externalconnector.zip``` for upload

## Spring profiles

Spring Profiles are used to activate different implementations of ConnectorLogging Beans. If you choose to use VS Code as your IDE, a lauch.json file is included that will make use of the "local" profile to print logs to the IDE output console.
