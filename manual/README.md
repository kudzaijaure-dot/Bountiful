# Step-By-Step Instructions to set up bountiful:

## 1: Create Contract
In an empty folder, open Visual Studio Code and creat an empty folder called client. This is where all our front-end code will reside.
![Screenshot (27)](https://user-images.githubusercontent.com/129856164/229991420-1698fcbb-999b-415b-abe7-372fa7571414.png)

You can then proceed to open the terminal, (without cd into client) and run the following command:
-- npx thirdweb@create --contract
This will initiate the build process as thirdweb constructs the necessary files and dependencies needed to build and deploy a smart contract easily. You should see something like this:
![Screenshot (29)](https://user-images.githubusercontent.com/129856164/229991828-55b0a8ce-7c51-4242-818f-074d0149a624.png)
![Screenshot (30)](https://user-images.githubusercontent.com/129856164/229991827-d205df81-a30d-49b8-b152-c6e6ec50f6c3.png)

For the first option, name your proect web3
![Screenshot (30)](https://user-images.githubusercontent.com/129856164/229992379-7c7e4a33-e010-488b-a904-029d0ead1837.png)


For the second option, select HardHat
![Screenshot (31)](https://user-images.githubusercontent.com/129856164/229992394-2739015b-05d5-4daf-9389-39f4d4056d47.png)


For the third option, name your contract anything you like, such as your company name or the purpose of the contract, in this case crowdfunding
![Screenshot (32)](https://user-images.githubusercontent.com/129856164/229992558-5779fcad-f1ba-4b3a-a2db-d83bb3b569ca.png)

For the final option, select EmptyContract, and let thirdweb start building your contract!
![Screenshot (33)](https://user-images.githubusercontent.com/129856164/229992691-91cd9db0-b94a-4215-a723-5e012695570b.png)

<br/>

## 2: Create MetaMask Wallet
This wallet is a crypt based wallet that serves as a wallet on any blockchain platform. This is what we will use to connect to our contract and our frontend, 
make transactions and deploy our contracts. 
To do so, visit: http://metamask.io/ and click the 'Add to Chrome Button'
![Screenshot (35)](https://user-images.githubusercontent.com/129856164/229993787-61671998-4d95-4c0c-9392-0e78a869f1d0.png)

After this step, follow the instructions to create a new account until you arrive at your main account page:
![Screenshot (38)](https://user-images.githubusercontent.com/129856164/229993933-1f90545e-a592-4a21-b8fa-9391a858309c.png)

Once your account has been set up, click on the top right bar where it says etherium mainnnet, select the drop down, select 'show/hide test networks' and set visibility 
for testnetworks to be on. This will allow us to connect to the Goerli tesnet and transact with GoerliEther, a testnet version of an etherium token.

![Screenshot (39)](https://user-images.githubusercontent.com/129856164/229994504-9bd251da-aa0f-4f64-991a-320465c62d98.png)

**Make sure to pin the metamask wallet to your browser toolbar for ease of access:**


Once this is complete, you should see a little orange fox at the top of your google chrome toolbar, and you can click on this to connect to Goerli. 
Select the button written 'Etherium Mainnet' and change the mainnet to the Goerli Testnet. (Upon production deployment, just do this in reverse).
You should see the results shown below:
![Screenshot (41)](https://user-images.githubusercontent.com/129856164/229995161-4d554043-3168-4174-bf4a-8787669b0bc1.png)

<br/>


## 3: Add Some GoerliETH to your wallet:
At the top of your wallet, youll see an address, this is your metamask wallet address. Click to copy this to your keyboard:
![Screenshot (44)](https://user-images.githubusercontent.com/129856164/229995713-bf00da64-c236-41a6-830f-754c7a080357.png)

Once complete, go to https://goerli-faucet.pk910.de/ to start mining some crypto! This faucet requires you to perform mining in return for some goerli tokens. 
Pass the Captcha test and past the copied address into the 'Address' bar and click start mining. This uses your computers processing resources to create
verify hashes for other developers in return for some Etherium. You can mine all you want for up to 5 hours, but the minimum claim reward is 0.001 GoerliETH
which takes at most 5 minutes of mining on a slow computer. Average testnets cost 0.05 GoerliETH (45 minutes - hour of mining) during non-peak times and around 
0.1 GoerliETH at peak times. 
![Screenshot (46)](https://user-images.githubusercontent.com/129856164/229996740-709969f5-19af-4d2b-a165-e34f724d80d5.png)
![Screenshot (47)](https://user-images.githubusercontent.com/129856164/229996820-65a10dc8-5f9a-4b1c-94fa-c63fcf279d3d.png)

**Leave this Process to run for a few hours**
<br/>
# 4: Setup Private Key
Go to your browser, click on the fox icon and open your wallet. 
Then, click on the dot menu to the top right of your main account page and select 'Account Details'

![Screenshot (48)](https://user-images.githubusercontent.com/129856164/229998780-200d9500-4ba4-4fa5-a0f6-16a797ef661b.png)

Then, select Export Private Key:

![Screenshot (49)](https://user-images.githubusercontent.com/129856164/229998863-bc2db64c-89be-4de5-a840-c72a93da44e7.png)

Copy the key you are given and reopen Visual Studio Code.

In the terminal, **navigate to your web3 folder with the cd web3 command** and then run the following command:
**npm install dotenv**
This is what we will use to store and access our private key in our project
Under the web3 folder, create a file called **.env** and write the following:

PRIVATE_KEY = XXXXX (Paste here)

Save the file and proceed with these instructions.
Open the HardHat.config.json file and paste the following code:
``` 
/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: {
    version: '0.8.9',
    defaultNetwork: 'goerli',
    networks: {
      hardhat: {},
      goerli: {
        url: 'https://rpc.ankr.com/eth_goerli',
        accounts: [`0x${process.env.PRIVATE_KEY}`]
      }
    },
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
};
```
This will ensure a connection to our metamask wallet and allow us to transact with our contracts.

<br/>

# 5: Designing Our Smart Contract:

In the web3/contracts folder, select and delete the contract.sol file, which is just a default contract web3 provides but in this case, we want to name our contract bountiful so create bountiful.sol
After doing so, copy and paste the following code:
```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;



contract bountiful {


  struct Campaign {
      address owner;
      string title;
      string description;
      uint256 target;
      uint256 deadline;
      uint256 amountCollected;
      string image;
      address[] donators;
      uint256[] donations;
  }

  mapping(uint256 => Campaign) public campaigns;

  uint256 public numberOfCampaigns = 0;

  function createCampaign(address _owner, string memory _title, string memory _description, uint256 _target, uint256 _deadline, string memory _image) public returns (uint256) {
      Campaign storage campaign = campaigns[numberOfCampaigns];

      require(campaign.deadline < block.timestamp, "The deadline should be a date in the future.");

      campaign.owner = _owner;
      campaign.title = _title;
      campaign.description = _description;
      campaign.target = _target;
      campaign.deadline = _deadline;
      campaign.amountCollected = 0;
      campaign.image = _image;

      numberOfCampaigns++;

      return numberOfCampaigns - 1;
  }

  function donateToCampaign(uint256 _id) public payable {
      uint256 amount = msg.value;

      Campaign storage campaign = campaigns[_id];

      campaign.donators.push(msg.sender);
      campaign.donations.push(amount);

      (bool sent,) = payable(campaign.owner).call{value: amount}("");

      if(sent) {
          campaign.amountCollected = campaign.amountCollected + amount;
      }
  }

  function getDonators(uint256 _id) view public returns (address[] memory, uint256[] memory) {
      return (campaigns[_id].donators, campaigns[_id].donations);
  }

  function getCampaigns() public view returns (Campaign[] memory) {
      Campaign[] memory allCampaigns = new Campaign[](numberOfCampaigns);

      for(uint i = 0; i < numberOfCampaigns; i++) {
          Campaign storage item = campaigns[i];

          allCampaigns[i] = item;
      }

      return allCampaigns;
  }
}
```

Save the file and reopen your terminal.
In your terminal, run: **npm run deploy**
Now we are attempting to deploy our contract to our testnet to test its function.
Wait until you receive a link to the thirdweb website where you can deploy your contract.

Connect your metamask wallet with the connect button shown, then under network configurations, select Goerli Testnet Under Testnets.
Once successful, click deploy.
You should Immediately see your metamask wallet pop-up, with a confirmation request to pay for the testnet. Click confirm and you should see this:

![Screenshot (22)](https://user-images.githubusercontent.com/129856164/230005108-bcf0c6ce-06db-4536-ae81-1064675d8d9a.png)

![Screenshot (23)](https://user-images.githubusercontent.com/129856164/230005107-dfc8b6d3-7f4d-4e3e-b5b4-50bf3db200c5.png)

Sign the request, and your contract will be successfully deployed!
You will then be redirected to your dashboard where you can manage your contract, view events and add more contract extensions.

![Screenshot (50)](https://user-images.githubusercontent.com/129856164/230005327-3348e39f-ee18-4cd5-8a36-77073497a395.png)

<br/>

# 6 Building The Front-End
The client side of our app will make use of react(vite) and tailwind css. To begin building our project:

**cd ../client**
And then run the following command:
**npx thirdweb@create --app**

Next, you will be prompted to make a series of selections. On the first option, name your project './' so that it takes the name of 
the folder (client) as its name. 
![Screenshot (54)](https://user-images.githubusercontent.com/129856164/230023418-9951e6c6-619d-4a19-b6ee-d688f302c1c7.png)

Next, select Vite as the program framework to use:
![Screenshot (56)](https://user-images.githubusercontent.com/129856164/230024408-5280ff8a-a020-47ce-860c-eeb6df330a4b.png)

Next, select JavaScript as the default language it will use.
![Screenshot (55)](https://user-images.githubusercontent.com/129856164/230023996-d8b89eb9-ab7d-4f52-a833-72d10d212512.png)

Next, you should see an output similar to the one below. 
![Screenshot (56)](https://user-images.githubusercontent.com/129856164/230023715-9b647256-09a8-45b4-acfd-a11453b42c8d.png)

Finally, install react-router-dom to enable navigability within our app:
Go to your terminal, ensure you are in the client folder and run the following command:
**npm install react-router-dom**
like so:
![Screenshot (58)](https://user-images.githubusercontent.com/129856164/230026483-8afef9f7-de18-46d5-8681-b9c258a655ba.png)


# 7: Adding assets and files
Now that our react app has been initialized, we can start implementing the front-end design and logic.
Navigate to this repositories **client/src** folder and add you can copy and paste the folder and files into your client folder as **src**.

In the code window, open src/context/index.jsx and navigate to the contract definition. In your browser, open your deployed contracts dashboard and copy the address under the contract name as so: 

![Screenshot (59)](https://user-images.githubusercontent.com/129856164/230028136-08bf556a-3748-4194-827a-b59754844cd8.png)

Take that value and paste it in index.jsx as shown below:
```
export const StateContextProvider = ({ children }) => {
  const { contract } = useContract('Enter your Metamask Address here');
  const { mutateAsync: createCampaign } = useContractWrite(contract, 'createCampaign');
```
This allows the front end to read from the contract we deployed, and our contract already reading our wallet information because remember, we used it to connect to the testnet, pay for the tesnet and got our private key that we placed in the .env file. 

# 8: Test and Deploy: 

To test the front-end and finally join the debugging process, run the following command:

**npm run dev**

This initiates a localhost instance that you can use to interact with your complete and connected system! (cntrl + c to cancel dev)
