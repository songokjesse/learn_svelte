<script context="module">
    // @ts-ignore
    export async function load({fetch, params}) {
        let id = params.id
        const res = await fetch(`https://jsonplaceholder.typicode.com/photos/${id}`)
        const photo = await res.json()

        if(res.ok) {
            return {
            props : {
                photo
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
    export let photo
</script>
<div>
    <h1>Photo</h1>
    <div>
        <h2>{photo.title}</h2>
        <img src={photo.url} alt=""/>
    </div>
</div>