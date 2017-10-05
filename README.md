*pwasm-test-tools* is a set of tools to make it easy to test pwasm-contracts written with [pwasm-std](https://github.com/NikVolf/pwasm-std).

## TODO
More examples and docs

## Usage

```rust

#[macro_use]
extern crate pwasm_std;
#[macro_use]
extern crate pwasm_test_tools;

use pwasm_std::{storage, ext};
use pwasm_std::hash::H256;
use pwasm_std::bigint::U256;

  #[cfg(test)]
    mod tests {
    use super::*;

    test_with_external!(
        DummyExternal: impl External for DummyExternal {

            fn storage_read(&mut self, key: &H256) -> Result<[u8; 32], Error> {
                Ok([1u8; 32])
            }
            fn value(&mut self) -> U256 {
                500.into()
            }
        }
        simple_test1 {
            let val = storage::read(&H256::from("68371d7e884c168ae2022c82bd837d51837718a7f7dfb7aa3f753074a35e1d87"));
            assert_eq!(val, Ok([1u8; 32]));
        }
        simple_test2 {
            assert_eq!(ext::value(), 500.into());
        }
    );

}
```

## Run

`rustup override set nightly`

`cargo test --features=std --verbose -- --nocapture`