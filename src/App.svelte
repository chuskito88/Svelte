<script>
	//Importamos la base de datos
	import { NotificationDisplay, notifier } from '@beyonk/svelte-notifications';
	import { db } from "./firebase";
	import {
		collection,
		getDocs,
		doc,
		addDoc,
		updateDoc,
		deleteDoc,
	} from "firebase/firestore";

	let n;
	let active = false;

	let cliente = {
		nombre: "",
		apellidos: "",
		horario: "",
		imagen: "",
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
	let message = "";

	const notify = async (message) => {
		notifier.danger(message, 7000); 
	};

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
		await updateDoc(doc(db, "clientes", cliente.id), cliente);
		await loadData();
		emptyform();
		message = "Cliente guardado";
		notify(message);
	};

	const editCliente = (c) => {
		cliente = Object.assign({}, c);
		editar = true;
		message = "Cliente a editar";
		notify(message);
	};

	const deleteClientes = async (id) => {
		await deleteDoc(doc(db, "clientes", id));
		await cargarClientes();
		emptyform();
		message = "Cliente borrado";
		notify(message);
	}; 

	const addClientes = async () => {
		await addDoc(collection(db, "clientes"), cliente);
		await cargarClientes();
		emptyform();
		message = "Cliente añadido";
		notify(message);
	}; 

	const onSubmitHandler = (e) => {
		if (editar) {
			console.log("Guardando...");
			saveCliente();
		} else {
			addClientes();
		}
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
		await updateDoc(doc(db, "monitores", monitore.id), monitor);
		await loadData();
		emptyform2();
		message = "Monitor guardado";
		notify(message);
	};

	const editMonitor = (m) => {
		monitor = Object.assign({}, m);
		editar = true;
		message = "Monitor editado";
		notify(message);
	};

	const deleteMonitor = async (id) => {
		await deleteDoc(doc(db, "monitores", id));
		await cargarMonitor();
		emptyform2();
		message = "Monitor borrado";
		notify(message);
	}; 

	const addMonitor = async () => {
		await addDoc(collection(db, "monitores"), monitor);
		await cargarMonitor();
		emptyform2();
		message = "Monitor añadido";
		notify(message);
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
	<div class="cabecera">
		<div class="titulo"><h1>GYM</h1></div>
	</div>
	<div class="clientes_print">
		{#each clientes as c, i}
			<div class="uno">
				{#if c.imagen}
					<img
						class="imagen_cl"
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
			</div>
		{/each}
	</div>

	<div class="form_cli">
		<form on:submit|preventDefault={onSubmitHandler}>
			<fieldset>Form Clientes</fieldset>
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

	</div>

	<div class="monitores_print">
		{#each monitores as m, i}
			<div class="dos">
				{#if m.imagen}
					<img
						class="imagen_cl"
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
			</div>
		{/each}
	</div>

	<div class="bot_mon">
		<button on:click="{() => active = !active}">
			Monitores
		</button>
	</div>

	

	<div class:active={!active} id="monito">
		<form on:submit|preventDefault={onSubmitHandler2}>
			<fieldset>Form Monitores</fieldset>
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

</style>
