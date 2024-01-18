# MNST-API

1) mnst/get_stakes?   - allows obtaining all stakes based on any of the three parameters or their combinations.

              pub_key - displays all stakes in a current masternode
              https://api-minter.mnst.club/mnst/get_stakes?pubkey=Mp46d3d6afe0084fcf530b03d1f4427e516a1cb4ec542640bcbc84c2c4b4f53c1
   
              coin - displays all stakes of a current coin.
              https://api-minter.mnst.club/mnst/get_stakes?coin=MNST

             address - finds all stakes of a current wallet.
             https://api-minter.mnst.club/mnst/get_stakes?address=Mx6a2a8335129e499fcd006da9ef1b9896b2c101fd

             pubkey=_&coin=_&address=_ - You can combine parameters in any convenient way. For example, you can get a list of stakes in the MonsterNode for the TAXFREE coin, or not specify any parameters at all to get a list of all stakes in all nodes. However, keep in mind that the latter option is resource-intensive and is often acceptable only for local use.
             https://api-minter.mnst.club/mnst/get_stakes?pubkey=Mp46d3d6afe0084fcf530b03d1f4427e516a1cb4ec542640bcbc84c2c4b4f53c13&coin=TAXFREE
             https://api-minter.mnst.club/mnst/get_stakes

2) mnst/height_by_time? - One of my favorite methods allows determining the block number that was a day or a week ago, or on January 1st at 00:00:01. The method frees up a considerable amount of resources needed to store the seemingly useless parameter of block creation time in its database and allows setting an index based on the block number. It is very useful for rankings and other services that require comparing data, for example, between the current and a day ago.

            https://api-minter.mnst.club/mnst/height_by_time?query=day
            https://api-minter.mnst.club/mnst/height_by_time?query=week
            https://api-minter.mnst.club/mnst/height_by_time?query=2024-01-01T00:00:01Z

3) mnst/address/ - Detailed balance of the wallet, taking into account all available types of coin holdings: FREEFLOAT, DELEGATE, WAITLIST, FROZEN, ORDERS, LIQUIDITY

            https://api-minter.mnst.club/mnst/address/Mxaa1eb05ade73bc78ad4828f8c78f84c3af4ec1ea

4) mnst/coins_info_by_ids? - Multi-query for coin information by their IDs. A very convenient method that allows obtaining comprehensive information about multiple coins with a single request.

            https://api-minter.mnst.club/mnst/get_coins_info?ids=40,50,80
   
5) mnst/coin_info? - A specialized method for obtaining information about a coin in terms of available types of coin holdings. For example, if you need to find out how much HUB is in liquidity pools or placed in orders, you will find this information in the query.

       https://api-minter.mnst.club/mnst/coin_info?symbol=HUB

       And if you need to find all liquidity pools that contain a specific coin, you can also obtain that information from the query.
       https://api-minter.mnst.club/mnst/coin_info?symbol=HUB&pools=true
   
6) mnst/coins_list - The method will output a list of all coins created in Minter with general information about the coins.
   
        https://api-minter.mnst.club/mnst/coins_list

7) mnst/rating - A query providing aggregate information about all nodes in the Minter network.
   
        https://api-minter.mnst.club/mnst/rating
   
8) mnst/pools_coin_list -  List and parameters of all LP-tokens.
   
        https://api-minter.mnst.club/mnst/pools_coin_list
