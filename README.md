# Wallet Adapter Test Project

See https://github.com/anza-xyz/wallet-adapter/pull/949

## Steps taken

1. Create a NextJS app with all defaults, just using npm:

```sh
$ npx create-next-app@latest
✔ What is your project named? … ledger-test
✔ Would you like to use TypeScript? … No / Yes (Yes)
✔ Would you like to use ESLint? … No / Yes (No)
✔ Would you like to use Tailwind CSS? … No / Yes (No)
✔ Would you like to use `src/` directory? … No / Yes (No)
✔ Would you like to use App Router? (recommended) … No / Yes (Yes)
✔ Would you like to customize the default import alias (@/*)? … No / Yes (No)
Creating a new Next.js app in ~/scratch/wallet-adapter-ledger-test/ledger-test.

Using npm.

<snip>

Success! Created ledger-test at ~/scratch/wallet-adapter-ledger-test/ledger-test
```

2. Install wallet-adapter dependencies:
```sh
$ npm install --save \
    @solana/wallet-adapter-base \
    @solana/wallet-adapter-react \
    @solana/wallet-adapter-react-ui \
    @solana/wallet-adapter-wallets \
    @solana/web3.js \
    react
```

3. Add a new component `app/client-layout.tsx`

(Equivalent of `pages/_app.tsx` without app directory)

```tsx
"use client";

import { WalletAdapterNetwork } from "@solana/wallet-adapter-base";
import { ConnectionProvider, WalletProvider } from "@solana/wallet-adapter-react";
import { WalletModalProvider } from "@solana/wallet-adapter-react-ui";
import { UnsafeBurnerWalletAdapter, LedgerWalletAdapter } from "@solana/wallet-adapter-wallets";
import { clusterApiUrl } from "@solana/web3.js";

// Default styles that can be overridden by your app
require('@solana/wallet-adapter-react-ui/styles.css');

export default function ClientLayout({
    children
}: Readonly<{
    children: React.ReactNode
}>) {
    const network = WalletAdapterNetwork.Devnet;
    const endpoint = clusterApiUrl(network);
    const wallets = [
        new UnsafeBurnerWalletAdapter(),
        new LedgerWalletAdapter(),
    ]

    return (
        <ConnectionProvider endpoint={endpoint}>
            <WalletProvider wallets={wallets} autoConnect>
                <WalletModalProvider>
                    {children}
                </WalletModalProvider>
            </WalletProvider>
        </ConnectionProvider>
    );
}
```

And then update `layout.tsx` to wrap childen in `ClientLayout`.

4. Add the `WalletMultiButton` to page.tsx

- Add `"use client";` at the top
- Replace description content:
```tsx
<div className={styles.description}>
    <WalletMultiButton />
</div>
```
