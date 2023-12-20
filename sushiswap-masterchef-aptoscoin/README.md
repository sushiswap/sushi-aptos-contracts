# Sushi Masterchef Smart Contract
Aptos Contract


## How to deploy sushiswap-masterchef contract on testnet

1. To deploy the contract we have to change the **masterchef_origin**,**msterchef_admin**,masterchef_upkeep_op
erator and **sushi_masterchef** address in the Move.toml
file.

<br>

2. To create **masterchef_origin**,**msterchef_admin** and
**masterchef_upkeep_operator** accounts run the below command.

    ```
    aptos init --profile masterchef_origin
    ```
   
    ```
    // Run this command only if you are not using multisig account other wise paste multisig account address in msterchef_admin
    
    aptos init --profile msterchef_admin
    ```
  
    ```
    aptos init --profile masterchef_upkeep_operator
    ```

<br>

3. To deploy sushiswap-masterchef contract run the below
command.

    ```
    aptos move create-resource-account-and-publish-package --seed 1223 --address-name sushi_masterchef
    ```
    
    <br/>

     ---
    **NOTE**

    After successfully deploying of contract replace sushi_masterchef address with the resource account address.

    ---

## How to run sushiswap-masterchef contract on testnet.

<br>

### Add New Pool Command - 


Run below command :

```
aptos move run --function-id sushi_masterchef(address)::masterchef::add_pool --profile msterchef_admin --args u64:1 bool:true(for regular farm value will be true and for spacia it will be false) bool:true
```

---
**NOTE**

Remove instruction which is given in parentheses.

if you're using normal admin account then use above command to add pool.

---

<br>

### Deposit Token

Run below command :

```
aptos move run --function-id sushi_masterchef(address)::masterchef::deposit --type-args “0x1::aptos_coin::AptosCoin(change address if you want to add other coin)” --args u64:100000000 (amount)
```

---
**NOTE**

Remove instruction which is given in parentheses.

---

<br>

### Upkeep

Run below command :

```
aptos move run --function-id sushi_masterchef(address)::masterchef::upkeep --profile masterchef_upkeep_operator --args u64:1000000000 (amount which you have to send to contract) u64:30 bool:true
```

---
**NOTE**

Remove instruction which is given in parentheses.

---


<br>

### Update Pool

Run below command :

```
aptos move run --function-id sushi_masterchef(address ::masterchef::update_pool --args u64:0 (your pool id)
```

---
**NOTE**

Remove instruction which is given in parentheses.

---


<br>

### Set Pool


Run below command :

```
aptos move run --function-id sushi_masterchef(address)::masterchef::set_pool --profile msterchef_admin --args u64:0 (your pool id) u64:2 (alloc point) bool:true
```

---
**NOTE**

Remove instruction which is given in parentheses.

if you're using normal admin account then use above command to set pool.

---
