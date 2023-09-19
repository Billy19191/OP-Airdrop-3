<script>
  import { browser } from '$app/environment'
  import { Alchemy, Network } from 'alchemy-sdk'

  const config = {
    apiKey: 'M_3gMwwnhUbhZpmi76kXHfGf9eQ2PNTC',
    network: Network.ETH_MAINNET,
  }
  const alchemy = new Alchemy(config)
  let dataLoad = false
  let data = []
  if (browser) {
    ;(async () => {
      try {
        const response = await fetch('/csvjson.json')
        data = await response.json()
        dataLoad = true
        console.log(data)
      } catch (error) {
        console.error('Error loading data:', error)
      }
    })()
  }
  let isInput
  let found, result
  let loading = false
  const searchFunc = async (event) => {
    loading = true
    isInput = event.target.value.length > 0 ? true : false
    let input = event.target.value
    if (input.endsWith('.eth')) {
      try {
        const resolvedAddress = await alchemy.core.resolveName(input)
        input = (await resolvedAddress) || input
      } catch (error) {
        console.error('Error resolving ENS name:', error)
        return
      }
    }
    loading = false
    console.log(input)
    result = data.find((each) => each.address === input.toLowerCase())
    found = result ? true : false
  }
</script>

{#if dataLoad}
  <div class="flex flex-col justify-center items-center mt-6 p-10 lg:px-64">
    <div class="flex justify-center font-bold text-[30px] lg:text-[40px]">
      Check OP Airdrop 3 Eligibility
    </div>
    <div class="flex flex-wrap mt-6">
      <div>
        Airdrop #3 is being directly disbursed to addresses that have delegated
        the voting power of their OP tokens between Jan 20 and July 20. On top
        of the baseline reward for delegation, addresses receive a 2x bonus if
        they have delegated to an active voter – someone who has participated in
        an onchain vote in Optimism governance. Read more about the airdrop <a
          class="inline-link text-red-600"
          href="https://community.optimism.io/docs/governance/airdrop-3/"
          target="_blank"
        >
          here
        </a>
      </div>
    </div>
  </div>
  <div>
    <div class="flex justify-center">
      <input
        placeholder="Enter your address"
        type="text"
        maxlength="42"
        class="border-2 border-black p-3 w-96 rounded-xl"
        on:input={searchFunc}
      />
    </div>
    {#if isInput}
      {#if loading}
        <div class="flex justify-center mt-10">Loading...</div>
      {:else if found}
        <div class="flex justify-center">
          <div class=" border-slate-300 border-2 rounded-xl m-10 w-[30em] p-10">
            <div class="text-[3.5em]">🥳</div>
            <div class="text-xl font-medium">
              You are eligible for airdrop 3!
            </div>
            <div class="flex mt-4">
              <div class=" italic text-[30px] font-bold w-fit">
                {result.total_op.toFixed(2)}
              </div>
              <div class="w-10">
                <img src="/optimism-logo.png" class="ml-3 w-fit h-10" />
              </div>
            </div>
            <div class="mt-4">
              Airdrop #3 is disbursed directly to eligible wallets. There is no
              need to claim tokens by interacting with any website.
            </div>
          </div>
        </div>
        <div />
      {:else if !found}
        <div class="flex justify-center">
          <div class=" border-slate-300 border-2 rounded-xl m-10 w-[30em] p-10">
            Sorry You Are not eligible :(
          </div>
        </div>
      {/if}
    {/if}
  </div>
{/if}
