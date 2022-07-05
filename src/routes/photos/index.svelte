<script context="module">
    export async function load({fetch}) {
        const res = await fetch('https://jsonplaceholder.typicode.com/photos')
        const photos = await res.json()

        if(res.ok) {
            return {
            props : {
                photos
            }
        }
        return {
           status: res.status,
           error: new Error("Photos not found")
        }
        
    }
}
</script>
<script>
// @ts-nocheck
   import { paginate, LightPaginationNav } from 'svelte-paginate'
   export let photos
    let items = photos
    let currentPage = 1
    let pageSize = 10
    $: paginatedItems = paginate({ items, pageSize, currentPage })
    let searchQuery = "" 
    $: searchedQuery = paginatedItems.filter((photo) => {
        return photo.title.includes(searchQuery)
    })
</script>

<div>
    <h1>Photos</h1>
    <br/>
    <input type="text" placeholder="Search" bind:value={searchQuery}/>
    <br/>
      
    <hr/>

    <ul>
        {#each searchedQuery as photo}
                    <li><a href={`/photos/${photo.id}`}>{photo.title}</a></li>
        {/each}
    </ul>

    <LightPaginationNav
    totalItems="{items.length}"
    pageSize="{pageSize}"
    currentPage="{currentPage}"
    limit="{1}"
    showStepOptions="{true}"
    on:setPage="{(e) => currentPage = e.detail.page}"
    />

</div>