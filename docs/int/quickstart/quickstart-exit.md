---
sidebar_position: 6
description: Exit a validator
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Exit a validator

:::caution
Charon is in an early alpha state and is not ready to be run on mainnet.
:::

Exiting your validator keys can be useful in situations where you want to stop staking and withdraw your staked ETH.

## Pre-requisites

- A quorum of operators needs to run the same exit command for the exit to succeed.
- If a charon client restarts after the exit command is run but before the threshold is reached, it will lose the partial exits it has stored. If all charon clients restart before the required threshold of exit messages are received, operators will have to rebroadcast the exit messages. 

## Step 1. Confirm the `EXIT_EPOCH`

Set the `EXIT_EPOCH` environment variable to the epoch number at which you want to exit. For example, if you want to exit at epoch 112260, enter the following command:
    
    export EXIT_EPOCH=112260

## Step 2. Run the `voluntary-exit` command

Run the following voluntarily exit the validator:

<Tabs groupId="validator-clients">
  <TabItem value="teku" label="Teku" default>
    <pre>
      <code>
    {String.raw`docker exec -ti charon-distributed-validator-node-teku-1 /opt/teku/bin/teku voluntary-exit \
    --beacon-node-api-endpoint="http://charon:3600/" \
    --confirmation-enabled=false \
    --validator-keys="/opt/charon/validator_keys:/opt/charon/validator_keys" \
    --epoch=$\{EXIT_EPOCH:-112260}`}
      </code>
    </pre>
  </TabItem>
  <TabItem value="lighthouse" label="Lighthouse">
    <pre>
      <code>
    {String.raw`docker exec -ti charon-distributed-validator-node-lighthouse-1 /opt/lighthouse/lighthouse validator exit \
    --beacon-node "http://charon:3600/" \
    --validator-dir "/opt/charon/validator_keys" \
    --epoch $\{EXIT_EPOCH:-112260}`}
      </code>
    </pre>
  </TabItem>
  <TabItem value="nimbus" label="Nimbus">
    <pre>
      <code>
    {String.raw`docker exec -ti charon-distributed-validator-node-nimbus-1 /opt/nimbus-eth2/build/nimbus_beacon_node validator_exit \
    --rpc="http://charon:3600/" \
    --validators-dir="/opt/charon/validator_keys" \
    --exit-epoch=$\{EXIT_EPOCH:-112260}`}
      </code>
    </pre>
  </TabItem>
</Tabs>

Once a threshold of exit signatures has been reached, charon will submit the exit transaction to the beacon chain.

## Feedback

If you have gotten this far through the process, and whether you succeeded or failed at exiting a validator, we would like to hear your feedback on the process and where you encountered difficulties. Please let us know by joining and posting on our [Discord](https://discord.gg/n6ebKsX46w). Also, feel free to add issues to our [GitHub repos](https://github.com/ObolNetwork).