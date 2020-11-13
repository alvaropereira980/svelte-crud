<script>
  import { v4 } from "uuid";
  import Noty from "noty";

  import "noty/lib/noty.css";
  import 'noty/lib/themes/sunset.css'

  let products = [
    {
      id: 1,
      name: "HP Paviliion Notebook",
      description: "HP Latop",
      category: "Laptop"
    },
    {
      id: 2,
      name: "Mouse",
      description: "HP Latop",
      category: "Laptop"
    },
    {
      id: 3,
      name: "Razer",
      description: "HP Latop",
      category: "Laptop"
    }
  ];

  let editStatus = false;

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageUrl: ""
  };

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: "",
      imageUrl: ""
    };
  };

  const addProduct = () => {
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageUrl: product.imageUrl
    };
    products = products.concat(newProduct);
    cleanProduct();
  };

  const updateProduct = () => {
    let updatedProduct = {
      name: product.name,
      description: product.description,
      id: product.id,
      imageUrl: product.imageUrl,
      category: product.category
    };

    const productIndex = products.findIndex(p => p.id === product.id);
    products[productIndex] = updatedProduct;
    cleanProduct();
    editStatus = false;
    new Noty({
      theme: "sunset",
      type: "success",
      timeout: 3000,
      text: "Product Updated Sucessfully"
    }).show();
  };

  const onSubmitHandler = e => {
    if (!editStatus) {
      addProduct();
    } else {
      updateProduct();
    }
  };
  const deleteProduct = id => {
    products = products.filter(product => product.id !== id);
  };

  const editProduct = productEdited => {
    product = productEdited;
    editStatus = true;
  };
</script>

<style>

</style>

<main>
  <div class="contaoner p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="car mt-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageUrl}
                  <img
                    src="images/no-product.png"
                    alt=""
                    class="img-fluid p-2" />
                {:else}
                  <img src={product.imageUrl} alt="" class="img-fluid p-2" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="car-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </h5>
                </div>
                <p class="card-text">{product.description}</p>
                <button
                  class="btn btn-danger"
                  on:click={deleteProduct(product.id)}>
                  Delete
                </button>
                <button
                  class="btn btn-secondary"
                  on:click={editProduct(product)}>
                  Edit
                </button>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandler}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  type="text"
                  placeholder="Product Name"
                  id="product-name"
                  class="form-control" />
              </div>
              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  id="product-desctiption"
                  cols="3"
                  placeholder="Product Description"
                  class="form-control" />
              </div>
              <div class="form-group">
                <input
                  bind:value={product.imageUrl}
                  type="url"
                  id="product-image-url"
                  placeholder="https://faztweb"
                  class="form-control" />
              </div>
              <div class="form-group">
                <select
                  id="category"
                  bind:value={product.category}
                  class="form-control">
                  <option value="laptop">Laptops</option>
                  <option value="peripherials">peripherials</option>
                  <option value="servers">servers</option>
                </select>
              </div>
              <button class="btn btn-secondary">
                {#if !editStatus}Save Product{:else}Update Product{/if}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
