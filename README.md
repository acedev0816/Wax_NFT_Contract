
# atomicpacks

The atomicpacks smart contract allows users to set up random pack openings, giving out [AtomicAssets] NFTs. It uses the [WAX RNG Oracle].
The smart contract is designed to be deployed only once and then used by different projects. It  has an internal RAM balance system that collections have to deposit RAM to, which is then used to pay for the RAM that is required when minting new NFTs.

## Functionalities

 1. Let the user select the pack NFT that they want to open, and then transfer the NFT to the atomicpacks contract with the memo `unbox`
 
 2. Repeatedly poll the `unboxassets` table with the scope being the asset_id of the transferred pack NFT. This will initially return no result, until the callback from the WAX RNG oracle is executed. This should usually only take a few seconds. Once there is data in this table, that is the result of the pack opening.

3. Let the user call the `claimunboxed` action so that they receive the NFTs.

4. Display the result to the user, using the template ids that were found in the `unboxassets` table.

Note that step 3 and 4 could also be swapped. We however believe that it is better to let the user call the claim action first, to prevent situations in which the user might exit the process before actually calling the claim action.

Also note that the `unboxpacks` table has a secondary index called `unboxer`. This can be used to detect any unclaimed results that the user might still have. \
(Reminder: The `unboxpacks` entry is erased once all results of that entry are claimed, so if there still is an entry in this table, you know that there must be unclaimed results)
