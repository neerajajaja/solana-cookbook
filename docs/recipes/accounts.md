---
title: Accounts
---

# Accounts

## Create a System Account

Create an account that the [System Program][1] owns. The Solana runtime will grant the owner of an account, access to
write to its data or transfer lamports. When creating an account, we have to preallocate a fixed storage space in bytes
(`space`) and enough lamports to cover the rent. [Rent][2] is a cost incurred to keep accounts alive on Solana.

<SolanaCodeGroup>
  <SolanaCodeGroupItem title="TS" active>

  <template v-slot:default>

@[code](@/code/accounts/create-system-account/create-system-account.en.ts)

  </template>

  <template v-slot:preview>

@[code](@/code/accounts/create-system-account/create-system-account.preview.en.ts)

  </template>

  </SolanaCodeGroupItem>
</SolanaCodeGroup>

## Calculating Rent Exemption

Keeping accounts alive on Solana incurs a storage cost called [rent][2]. An account can be made entirely exempt
from rent collection by depositing at least two years worth of rent. For the calculation, you need to consider
the amount of data you intend to store in the account.

<CodeGroup>
  <CodeGroupItem title="TS" active>

@[code](@/code/accounts/rent-exemption/rent-exemption.en.ts)

  </CodeGroupItem>

  <CodeGroupItem title="CLI">

@[code](@/code/accounts/rent-exemption/rent-exemption.en.sh)

  </CodeGroupItem>
</CodeGroup>

## Program Derived Address

[Program derived address(PDA)][3] is like a normal address with the following differences:

1. Falling off ed25519 curve
2. Using program to sign instead of private key

::: tip
Although PDA is derived by a program id, it doesn't means the PDA is owned by the same program. (Take an example, you can initialize your PDA as a token account which is an account owned by token program)
:::

### Generate a PDA

1. Create Program Address

This may fail because the result (pda) is on curve. You can use 
`findProgramAddress` to reserve your meaningful seed.

<CodeGroup>
  <CodeGroupItem title="TS" active>

@[code](@/code/accounts/program-derived-address/derived-a-pda/create-program-address.en.ts)

  </CodeGroupItem>
</CodeGroup>

2. Find Program Address

`findProgramAddress` will add a extra byte at the end of your seed. 
It starts from 255 to 0 and returns the first off-curve public key. 
You will always get the same result if you pass the same program id 
and seed.

<CodeGroup>
  <CodeGroupItem title="TS" active>

@[code](@/code/accounts/program-derived-address/derived-a-pda/find-program-address.en.ts)

  </CodeGroupItem>
</CodeGroup>

### Sign with a PDA

PDAs can only be signed for within the program. Below is a program 
example of signing with a PDA and calling the program with the client.

#### Program

The below shows a single instruction that transfers SOL from a PDA that
was derived by the seed `escrow` to an account passed. `invoke_signed` is
used to sign with the PDA.

<SolanaCodeGroup>
  <SolanaCodeGroupItem title="rust" active>

  <template v-slot:default>

@[code](@/code/accounts/program-derived-address/sign-a-pda/program/src/lib.rs)

  </template>

  <template v-slot:preview>

@[code](@/code/accounts/program-derived-address/sign-a-pda/program/src/lib.preview.rs)

  </template>

  </SolanaCodeGroupItem>
</SolanaCodeGroup>

#### Client

<SolanaCodeGroup>
  <SolanaCodeGroupItem title="TS" active>

  <template v-slot:default>

@[code](@/code/accounts/program-derived-address/sign-a-pda/client/main.en.ts)

  </template>

  <template v-slot:preview>

@[code](@/code/accounts/program-derived-address/sign-a-pda/client/main.preview.en.ts)

  </template>

  </SolanaCodeGroupItem>
</SolanaCodeGroup>

## Get Program Accounts

Return all accounts owned by a program. Refer to the [ingredients section](../ingredients/get-program-accounts.md) for more information on `getProgramAccounts` and its configuration.

<CodeGroup>
  <CodeGroupItem title="TS" active>

@[code](@/code/get-program-accounts/basic/basic.en.ts)

  </CodeGroupItem>

  <CodeGroupItem title="CLI">

@[code](@/code/get-program-accounts/basic/basic.en.sh)

  </CodeGroupItem>
</CodeGroup>

[1]: https://docs.solana.com/developing/clients/javascript-reference#systemprogram
[2]: https://docs.solana.com/developing/programming-model/accounts#rent
[3]: https://docs.solana.com/developing/programming-model/calling-between-programs#program-derived-addresses
