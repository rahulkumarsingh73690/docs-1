# Writing our Contract

Now that we've figured out the specification, we can start implementing our contract in code.

## Start with a template

In your working directory, you'll want to use `cargo-generate` to start your smart contract with the recommended folder structure and build options:

```sh
cargo generate --git https://github.com/CosmWasm/cosmwasm-template.git --name my-terra-token
```

## Implementing messages

We'll first start by defining our messages using `struct`. This defines a data structure that will represent our message -- which is intended to be passed from the user to blockchain nodes through JSON.

#### src/msg.rs

Each data structure will be needed to be prefixed by a long derive macro which will provide implementations for traits to make it simpler for serialization between JSON and Rust and debugging.

- `Serialize`: add serializer
- `Deserialize`: add deserializer
- `Clone`: enable struct to be cloned
- `PartialEq`: allow struct to be compared for equality
- `JsonSchema`: generates JSON schema

::: warning NOTE
`HumanAddr` here refers to the "human-readable" `terra-` account address.
:::

```rust
use schemars::JsonSchema;
use serde::{Deserialize, Serialize};

use cosmwasm_std::{HumanAddr, Uint128};

#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct InitialBalance {
    pub address: HumanAddr,
    pub amount: Uint128,
}

#[derive(Serialize, Deserialize, JsonSchema)]
pub struct InitMsg {
    pub name: String,
    pub symbol: String,
    pub decimals: u8,
    pub initial_balances: Vec<InitialBalance>,
}
```

As for our `HandleMsg`, we will use an enum to multiplex over the different types of messages that our contract can understand. We use the `serde` macro to rewrite the attribute key names to use snake-case, so we'll have `transfer_from` instead of `TransferFrom`.

```rust
#[derive(Serialize, Deserialize, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
  Approve {
    spender: HumanAddr,
    amount: Uint128,
  },
  Transfer {
    recipient: HumanAddr,
    amount: Uint128,
  },
  TransferFrom {
    owner: HumanAddr,
    recipient: HumanAddr,
    amount: Uint128,
  },
  Burn {
    amount: Uint128,
  },
}
```

For our `QueryMsg`, we'll have to also define the structure of the responses because `query()` will send back information back to the user through JSON.

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
  Balance {
    address: HumanAddr,
  },
  Allowance {
    owner: HumanAddr,
    spender: HumanAddr,
  },
}

#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct BalanceResponse {
    pub balance: Uint128,
}

#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct AllowanceResponse {
    pub allowance: Uint128,
}
```

## Main contract logic

After we've defined our messages, we can move on to the main contract logic.

#### src/contract.rs

```rust

```
