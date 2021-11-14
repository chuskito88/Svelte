<script>
	//Importamos la base de datos
	import { db } from "./firebase";
	import {
		collection,
		getDocs,
		doc,
		addDoc,
		updateDoc,
		deleteDoc,
	} from "firebase/firestore";

	let active = false;

	let cliente = {
		nombre: "",
		apellidos: "",
		horario: "",
		imagen: "",
		monitor: "",
		edad: "",
	};
	let monitor = {
		nombre: "",
		apellidos: "",
		horario: "",
		imagen: "",
		edad: "",
	};

	let clientes = [];

	let monitores = [];


	let editar = false;

	//FUNCIONES PARA CLIENTES
	const cargarClientes = async () => {
		const querySnapshot = await getDocs(collection(db, "clientes"));
		let listado = [];
		querySnapshot.forEach((lista) => {
			listado.push({ ...lista.data(), id: lista.id });
		});
		clientes = [...listado];
		console.log(clientes);
	};
	cargarClientes();

	const emptyform = async () => {
		cliente = {
			nombre: "",
			apellidos: "",
			horario: "",
			imagen: "",
			edad: "",
		};
		editar = false;
	}

	const saveCliente = async () => {
		await updateDoc(doc(db, "clientes", product.id), product);
		await loadData();
		emptyform();
	};

	const editCliente = (c) => {
		cliente = Object.assign({}, c);
		editar = true;
	};

	const deleteClientes = async (id) => {
		await deleteDoc(doc(db, "clientes", id));
		await cargarClientes();
		emptyform();
	}; 

	const addClientes = async () => {
		await addDoc(collection(db, "clientes"), cliente);
		await cargarClientes();
		emptyform();
	}; 

	const onSubmitHandler = (e) => {
		if (editar) {
			// Guardamos
			console.log("Guardando...");
			saveCliente();
		} else {
			addClientes();
		}
		addClientes();
	};



	//FUNCIONES PARA MONITORES
	const cargarMonitor = async () => {
		const querySnapshot = await getDocs(collection(db, "monitores"));
		let listado = [];
		querySnapshot.forEach((lista) => {
			listado.push({ ...lista.data(), id: lista.id });
		});
		monitores = [...listado];
		console.log(monitores);
	};
	cargarMonitor();

	const emptyform2 = async () => {
		monitor = {
			nombre: "",
			apellidos: "",
			horario: "",
			imagen: "",
			edad: "",
		};
		editar = false;
	}
	
	const saveMonitor = async () => {
		await updateDoc(doc(db, "monitores", product.id), product);
		await loadData();
		emptyform2();
	};

	const editMonitor = (m) => {
		monitor = Object.assign({}, m);
		editar = true;
	};

	const deleteMonitor = async (id) => {
		await deleteDoc(doc(db, "monitores", id));
		await cargarMonitor();
		emptyform2();
	}; 

	const addMonitor = async () => {
		await addDoc(collection(db, "monitores"), monitor);
		await cargarMonitor();
		emptyform2();
	}; 

	const onSubmitHandler2 = (e) => {
		if (editar) {
			// Guardamos
			console.log("Guardando...");
			saveMonitor();
		} else {
			addMonitor();
		}
		addMonitor();
	};

</script>

<main>
	<div>
		{#each clientes as c, i}
			{#if c.imagen}
				<img
					src={c.imagen}
					alt="thumbnail"
				/>
			{:else}
				<p
					No encontrada
				/>
			{/if}

			<p>Nombre: {c.nombre}</p>
			<p>Apellidos: {c.apellidos}</p>
			<p>Horario: {c.horario}</p>
			<p>Edad: {c.edad}</p>
			<button on:click="{editCliente(c)}">Editar</button>
			<button on:click="{deleteClientes(c.id)}">Eiminar</button>
			<br>
		{/each}
	</div>

	<div>
		{#each monitores as m, i}
			{#if m.imagen}
				<img
					src={m.imagen}
					alt="thumbnail"
				/>
			{:else}
				<p
					No encontrada
				/>
			{/if}

			<p>Nombre: {m.nombre}</p>
			<p>Apellidos: {m.apellidos}</p>
			<p>Horario: {m.horario}</p>
			<p>Edad: {m.edad}</p>
			<button on:click="{editMonitor(m)}">Editar</button>
			<button on:click="{deleteMonitor(m.id)}">Eiminar</button>
			<br>
		{/each}
	</div>

	<div>
		<h1>Form Cliente</h1>
		<form on:submit|preventDefault={onSubmitHandler}>
			<p>Nombre</p>
			<input type="text" bind:value={cliente.nombre} />
			<p>Apellidos</p>
			<input type="text" bind:value={cliente.apellidos} />
			<p>Edad</p>
			<input type="text" bind:value={cliente.edad} />
			<p>Horario</p>
			<select bind:value={cliente.horario}>
				<option>Mañana</option>
				<option>Tarde</option>
			</select>
			<p>Imagen</p>
			<input type="text" bind:value={cliente.imagen} />
			<br>
			{#if editar}
				<button>
					Editar
				</button>
			{:else}
				<button>
					Añadir
				</button>
			{/if}
			
		</form>

		<button on:click="{() => active = !active}">
			Monitores
		</button>
	</div>

	<div class:active={active} id="monito">
		<h1>Form Monitor</h1>
		<form on:submit|preventDefault={onSubmitHandler2}>
			<p>Nombre</p>
			<input type="text" bind:value={monitor.nombre} />
			<p>Apellidos</p>
			<input type="text" bind:value={monitor.apellidos} />
			<p>Edad</p>
			<input type="text" bind:value={monitor.edad} />
			<p>Horario</p>
			<select bind:value={monitor.horario}>
				<option>Mañana</option>
				<option>Tarde</option>
			</select>
			<p>Imagen</p>
			<input type="text" bind:value={monitor.imagen} />
			<br>
			{#if editar}
				<button>
					Editar
				</button>
			{:else}
				<button>
					Añadir
				</button>
			{/if}
		</form>
	</div>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	img{
		width: 300px;
		height: 350px;
	}
	.active{
		display: none;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>