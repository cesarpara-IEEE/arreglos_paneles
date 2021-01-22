<script>

	import Flex from 'svelte-flex';

	let vol = 0
	let vol_panel = 0
	let n_string = 0
	let n_mppt = 0
	let n_panel = 0

	$: lado1 = parseInt(Math.sqrt(n_panel))
	$: lado2 = parseInt(n_panel/lado1)
	$: sobra = n_panel - lado1*lado2	
	$: max_mppt = unos(n_mppt)
	$: min_mppt = unos(n_mppt)
	$: series_max = parseInt(vol/vol_panel)
	$: resto = n_panel - mulsuma(max_mppt, min_mppt)

	function enumeracion (a){
		let lista = []
		for(var n=0; n<a; n++){
			lista.push(n)
		}
		return lista
	}

	function ceros(a){
		let lista = []
		for(var n=0; n<a; n++){
			lista.push(0)
		}
		return lista
	}

	function unos(a){
		let lista = []
		for(var n=0; n<a; n++){
			lista.push(1)
		}
		return lista
	}

	function mulsuma(lis1, lis2){
		let acum = 0
		for (let index = 0; index < lis1.length; index++) {
			acum += lis1[index]*lis2[index]
		}
		return acum
	}
</script>

<label for="">Voltaje maximo:</label> 
<input bind:value={vol}>

<label for="">Voltaje de panel:</label>
<input bind:value={vol_panel}>

<label for="">Numero de strings por mppt:</label>
<input bind:value={n_string}>

<label for="">Numero de mppts:</label>
<input bind:value={n_mppt}>

<label for="">Numero de paneles fotovoltaicos:</label>
<input bind:value={n_panel}>



{#each enumeracion(lado1) as num }
	<Flex justify = 'start'>
		{#each enumeracion(lado2) as num}
		<div class="box"></div>
		{/each}
	</Flex>
{/each}
<Flex justify = 'start'>
	{#each enumeracion(sobra) as num}
	<div class="box"></div>
	{/each}
</Flex>		


<Flex justify = 'start'>
	{#each enumeracion(n_mppt) as i}
	<div class="mppt">
		<Flex direction = 'column'>
			<Flex justify = 'start'>
				<div>
					<label for="">Series {max_mppt[i]}</label>
					<input type="range" bind:value={max_mppt[i]} max={series_max}>			
				</div>
			</Flex>
			<Flex justify = 'start'>
				<div>
					<label for="">Paralelos	{min_mppt[i]}</label>
					<input type="range" bind:value={min_mppt[i]} max={n_string}>
				</div>
			</Flex>
			{#each enumeracion(min_mppt[i]) as num }
				<Flex justify = 'start'>
					{#each enumeracion(max_mppt[i]) as num}
					<div class="boxmin"></div>
					{/each}
				</Flex>
			{/each}	
			<label for="">V: {vol_panel*max_mppt[i]}</label>
		</Flex>
	</div>

	{/each}
</Flex>

<label for="">Numero de paneles sobrantes: {resto}</label>

<style direction = "row">
	.box{
		margin: 1px;
		width: 10px;
		height: 10px;
		background-color: orange;
	}

	.boxmin{
		margin: 0.5px;
		width: 5px;
		height: 5px;
		background-color: green;
	}

	.mppt{
		margin: 5px;
	}
</style>