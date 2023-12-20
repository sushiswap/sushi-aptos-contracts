# Sushi Swap Contracts
Aptos contracts

## Dependence
Aptos CLI

## How to use

1. Initialize your aptos account
    ```shell
    $ aptos  init
    ```
you will get a ".aptos" folder in your current folder.

```shell
    config.yaml
    profiles:
      default:
        private_key: "0x0000000000000000000000000000000000000000000000000000000000000000"
        public_key: "0x0000000000000000000000000000000000000000000000000000000000000000"
        account: 3add3576f7f3f411a5bd5fbab22dff4747107f25ce8726bf9926542718ff8a26   # your_original_account
        rest_url: "https://fullnode.devnet.aptoslabs.com/v1"
        faucet_url: "https://faucet.devnet.aptoslabs.com/"
```
2. Get test APT

    ```shell
      $ aptos account  fund-with-faucet --account your_original_account --amount 100000000
    ```
3. Create your resource account

    ```shell
    $ aptos move run --function-id '0x1::resource_account::create_resource_account_and_fund' --args 'string:sushi' 'hex:your_original_account' 'u64:10000000'
    ```
4. Get your resourc eaccount 
    ```shell
    $ aptos account list --account your_original_account
    ```

Or find it on explorer: https://explorer.devnet.aptos.dev/account/your_original_account

  ```txt
      TYPE:
      0x1::resource_account::Container
      DATA:
      {
        "store": {
          "data": [
            {
              "key": "0x929ac1ea533d04f7d98c234722b40c229c3adb1838b27590d2237261c8d52b68",
              "value": {
                "account": "0x929ac1ea533d04f7d98c234722b40c229c3adb1838b27590d2237261c8d52b68"  # your_resource_account
              }
            }
          ]
        }
      }
  ```
5. Add the resource account in config.yaml

  ```shell
    profiles:
      default:
        private_key: "0x0000000000000000000000000000000000000000000000000000000000000000"
        public_key: "0x0000000000000000000000000000000000000000000000000000000000000000"
        account: 3add3576f7f3f411a5bd5fbab22dff4747107f25ce8726bf9926542718ff8a26   # your_original_account
        rest_url: "https://fullnode.devnet.aptoslabs.com/v1"
        faucet_url: "https://faucet.devnet.aptoslabs.com/"
      sushi:
        private_key: "0x0000000000000000000000000000000000000000000000000000000000000000" # same as above
        public_key: "0x0000000000000000000000000000000000000000000000000000000000000000" # same as above
        account: 929ac1ea533d04f7d98c234722b40c229c3adb1838b27590d2237261c8d52b68   # your_resource_account
        rest_url: "https://fullnode.devnet.aptoslabs.com/v1"
        faucet_url: "https://faucet.devnet.aptoslabs.com/"

  ```


6. Edit Move.toml file

      ```shell
    [package]
    name = "SushiSwap"
    version = "0.0.1"
    [dependencies]
    AptosFramework = { git = "https://github.com/aptos-labs/aptos-core.git", subdir = "aptos-move/framework/aptos-framework/", rev = "72421d32d77f1877ded478e96f5b95914de1df91" }
    AptosStdlib = { git = "https://github.com/aptos-labs/aptos-core.git", subdir = "aptos-move/framework/aptos-stdlib/", rev = "72421d32d77f1877ded478e96f5b95914de1df91" }
    [addresses]
    sushi = "71e609393d30dfacaf477c9a9cd7824ae14b5f8d2a20c0b1917325d41e4a4aac" //repalce this with your_resource_account 
    dev = "2e5cc2bff22d15be32613aace67b7386251b8ae808a99241ee34b4703f780e2c" // repalce this with your_original_account which you created the resource account 
    zero = "0000000000000000000000000000000000000000000000000000000000000000"
    #Use multi sign address as a default admin
    default_admin = "0000000000000000000000000000000000000000000000000000000000000000" // need to create an admin account, and replace this.
    ``` 
7. Compile code
    ```shell
    $ aptos move compile
    ```
8. Publish package
      ```shell
      $ aptos move publish --profile sushi
      ```

9. Withdraw Fee 

    ```
    $ aptos move run --function-id "0x9adfcc0f47c006ebccad98109a33986280bbdef82a212ae15c97292a2981d9e1::swap::withdraw_fee" --type-args "0x5728d69f8a1c64b4cdb59f7746fe1a847215716e77c4bc90128c1100da826946::matic::Polygon" "0xe05d610ddad41a45e61b1327f01fcc0a582eedae00683ef969b85fa892c4b4f::usdt::Tether" --profile FEE_TO_ACCOUNT

    ```