<script>
  async function createProtocol(protocolData) {
    console.log('Sending request to create protocol:', protocolData);
    const response = await fetch('http://localhost:9000/procotols/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(protocolData),
    });

    if (!response.ok) {
      const message = `An error has occured: ${response.status}`;
      throw new Error(message);
    }

    const data = await response.json();
    return data;
  }

  function addReagent() {
    const reagentsSection = document.getElementById('reagentsSection');
    const div = document.createElement('div');
    div.innerHTML = `
      <label for="reagentName">Nom du réactif:</label><br>
      <input type="text" id="reagentName" name="reagentName">*
      <label for="reagentPrice">Prix:</label><br>
      <input type="text" id="reagentPrice" name="reagentPrice">
      <label for="reagentQuantity">Quantité:</label><br>
      <input type="number" id="reagentQuantity" name="reagentQuantity">

    `;
    reagentsSection.appendChild(div);
  }

  function addConsumable() {
    const consumablesSection = document.getElementById('consumablesSection');
    const div = document.createElement('div');
    div.innerHTML = `
      <label for="consumableName">Nom du consommable:</label><br>
      <input type="text" id="consumableName" name="consumableName">
      <label for="consum">Quantité:</label><br>
      <input type="number" id="consum" name="consum">
      <label for="consumableUnit">Unité:</label><br>
      <input type="text" id="consumableUnit" name="consumableUnit">
    `;
    consumablesSection.appendChild(div);
  }

  function handleClick() {
    const tab = document.createElement('div');
    tab.style.border = '1px solid black';
    tab.style.padding = '10px';
    tab.style.marginTop = '10px';
    const form = document.createElement('form');

    form.innerHTML = `
  <label for="name">Nom du protocole:</label><br>
  <input type="text" id="name" name="name"><br>
  
  <div id="reagentsSection">
    <label for="reagentName">Nom du réactif:</label><br>
    <input type="text" id="reagentName" name="reagentName">
    <label for="reagentPrice">Prix:</label><br>
    <input type="text" id="reagentPrice" name="reagentPrice">
    <label for="reagentQuantity">Quantité:</label><br>
    <input type="number" id="reagentQuantity" name="reagentQuantity">
    <button type="button" onclick="addReagent()">Ajouter Réactif</button>
  </div>
  
  <div id="consumablesSection">
    <label for="consumableName">Nom du consommable:</label><br>
    <input type="text" id="consumableName" name="consumableName">
    <label for="consumablePrice">Prix:</label><br>
    <input type="number" id="consumablePrice" name="consumablePrice">
    <label for="consumableQuantities">QUantité:</label><br>
    <input type="text" id="consumableQyantities" name="consumableQuantities">
    <button type="button" onclick="addConsumable()">Ajouter Consommable</button>
  </div>
  
  <input type="submit" value="Créer un protocole">
`;

    form.addEventListener('submit', async function(event) {
      event.preventDefault();

      const name = document.getElementById('name').value;

      const reagentNames = document.getElementsByName('reagentName');
      const reagentQuantities = document.getElementsByName('reagentQuantity');
      const reagentPrice = document.getElementsByName('reagentPrice');

      const reagentList = [];
      for (let i = 0; i < reagentNames.length; i++) {
        const name = reagentNames[i].value.trim();
        const quantity = reagentQuantities[i].value.trim();
        const price = reagentPrice[i].value.trim();
        reagentList.push({ name, price, quantity });
      }

      const consumableNames = document.getElementsByName('consumableName');
      const consumablePrices = document.getElementsByName('consumablePrice');
      const consumableQuantities = document.getElementsByName('consumableQuantities');

      const consumableList = [];
      for (let i = 0; i < consumableNames.length; i++) {
        const name = consumableNames[i].value.trim();
        const price = consumablePrices[i].value.trim();
        const quantity = consumableQuantities[i].value.trim();
        consumableList.push({ name, price, quantity});
      }

      const protocolData = {
        name: name,
        reagentList: reagentList,
        consumableList: consumableList
      };

      try {
        const createdProtocol = await createProtocol(protocolData);
        console.log('Protocol created:', createdProtocol);
      } catch (error) {
        console.error('Error creating protocol:', error);
      }
    });

    tab.appendChild(form);
    const tabsContainer = document.getElementById('tabsContainer');
    if (tabsContainer) {
      tabsContainer.appendChild(tab);
    }
  }

  function remove() {
    const tabs = document.getElementById('tabsContainer');
    if (tabs) {
      tabs.innerHTML = '';
    }
  }

  function updateProtocol() {
    console.log("update protocol"); 
  }

</script>

<body>
    <h1>Welcome on Budget App {name}  </h1> 
    <div class="button-container">
        <button id="myButton" on:click={handleClick}>
            Create Protocol
        </button>

        <button on:click={updateProtocol}>
            Update Protocol
        </button>
        
        <button on:click={remove}> 
            Reset la page 
        </button>
    </div>

    <div id="tabsContainer">
        <!-- Tabs will be added here -->
    </div>

    <div class="footer">
        <p>Entreprise: Fixgia</p>
        <p>Adresse: 123 Rue du Code, Ville, Pays</p>
        <p>Téléphone: +1 234 567 890</p>
        <p>Email: info@fixgia.com</p>
    </div>
</body>
<style> 
    body {
    width: 1600px;
    margin: 0 auto;
    }

    h1 {
        text-align: center;
        font-style: italic;
        margin-bottom: 50px;
    }
    .button-container {
        display: flex;
        justify-content: center;
    }
    button {
        background-color: grey;
        border: 2px solid black;
        color: black;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 20px 18px;
        cursor: pointer;
        border-radius: 12px;
    }

    .footer {
        text-align: center;
        background-color: lightgray;
        padding: 10px;
        margin-top: 20px;
        border-radius: 5px;
    }

</style>

