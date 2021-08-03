<script>
	import { v4 } from 'uuid';
	import Noty from 'noty';

	import 'noty/lib/noty.css';
	import 'noty/lib/themes/sunset.css';

	let products = [
    {
      id: 1,
      name: "Hp Pavilion Notebook",
      description: "HP Laptop",
      category: "laptops"
    },
    {
      id: 2,
      name: "Razer Mouse",
      description: "Gaming Mouse",
      category: "peripherals"
    },
    {
      id: 3,
      name: "Corsair Keyboard",
      description: "Gaming keyboard",
      category: "peripherals"
    }
  ];

	// variable para el producto
	let product = {
		id: "",
		name: "",
		description: "",
		category: "",
		imageURL: "",
	};

	let editStatus = false;

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: "",
      imageURL: ""
    };
  };

  const addProduct = () => {
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    };
    products = products.concat(newProduct);

    	cleanProduct();
    	console.log(products);
  };


  const deleteProduct = id => {
    console.log(id);
    products = products.filter(product => product.id !== id);
  };
  const editProduct = productEdited => {
    editStatus = true;
    product = productEdited;
    console.log(product);
  };

	const updateProduct = () => {
		let updatedProduct = {
			name: product.name,
			description: product.description,
			id: product.id,
			imageURL: product.imageURL,
			category: product.category
		}

		const productIndex = products.findIndex(p => p.id === product.id);
		products[productIndex] = updatedProduct;
		cleanProduct();
		editStatus = false;
		new Noty({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			text: 'Su producto fue actualizado correctamente'
		}).show();
	};

	const onSubmintHandler = () => {
		if (!editStatus) {
			addProduct();
		} else {
			updateProduct();
		}
	};
</script>

<main>
	<div class="container p-4">
		<div class="row">
			<div class="col-md-6">
				{#each products as product}
					<div class="card mt-2">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageURL}
									<img
										src="images/notfound.png"
										alt=""
										class="img-fluid p-2"
									/>
								{:else}
									<img
										src={product.imageURL}
										alt=""
										class="img-fluid p-2"
									/>
								{/if}
							</div>
							<div class="col-md-8">
								<div class="card-body">
									<h5>
										<strong>{product.name}</strong>
										<span>
											<small>
												{product.category}
											</small>
										</span>
									</h5>
									<p class="card-text">
										{product.description}
									</p>
									<button
										class="btn btn-secondary disabled"
										on:click={editProduct(product)}
									>
										edit
									</button>
									<button
										class="btn btn-danger"
										on:click={deleteProduct(product.id)}
									>
										delete
									</button>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>

			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmintHandler}>
							<div class="form-group">
								<input
									bind:value={product.name}
									type="text"
									placeholder="Product name"
									id="product-name"
									class="form-control"
								/>
							</div>

							<div class="form-group">
								<textarea
									bind:value={product.description}
									id="product-description"
									rows="3"
									placeholder="product-description"
									class="form-control"
								/>
							</div>

							<div class="form-group">
								<input
									bind:value={product.imageURL}
									type="url"
									id="product-image-url"
									placeholder="https://soluslab.com"
									class="form-control"
								/>
							</div>

							<div class="form-group">
								<select
									bind:value={product.category}
									id="category"
									class="form-control"
								>
									<option value="laptops"> Laptops </option>
									<option value="peripherials">
										Perifericos
									</option>
									<option value="servers">
										Servidores
									</option>
								</select>
							</div>

							<button class="btn btn-secondary">
								{#if !editStatus}
									Save product
								{:else}
									Update product
								{/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>

<style>
</style>
