<script>
  import { browser } from '$app/environment'
  let data_new = []
  if (browser) {
    ;(async () => {
      let data = await fetch('/csvjson.json')
      data_new = await data.json()
      console.log(data_new)
    })()
  }
  let result
  const searchFunc = async (event) => {
    let input = event.target.value
    result = await data_new.find((each) => each.address === input)
    if (result) {
      console.log(result)
    } else {
      console.log('not found')
    }
  }
</script>

<div
  class="flex flex-col justify-center items-center font-bold text-[50px] mt-6"
>
  <div class="flex justify-center">Check your airdrop 3 eligibly</div>
</div>
<div class="p-10">
  <input
    type="text"
    maxlength="42"
    class="border-2 border-black p-3 w-full"
    on:input={searchFunc}
  />
</div>

{#if result}
  <div>Your are eligible for airdrop 3!</div>
  <div>{result.total_op}</div>
{:else}
  <div>Sorry You Are not eligible :(</div>
{/if}
