<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form class="burgerForm" @submit="createBurger">
                <div class="inputContainer">
                    <label for="name">Nome do Cliente:</label>
                    <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome">
                </div>
                <div class="inputContainer">
                    <label for="bread">Escolha o pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                        <option value=""> Selecione o seu pão</option>
                        <option v-for="bread in breads" :key="bread.id" :value="bread.type">{{ bread.type }}</option>
                    </select>
                </div>
                <div class="inputContainer">
                    <label for="meat">Escolha o tipo de carne:</label>
                    <select name="meat" id="meat" v-model="meat">
                        <option value=""> Selecione sua carne</option>
                        <option v-for="meat in meats" :key="meat.type" :value="meat.type">{{ meat.type }}</option>
                    </select>
                </div>
                <div class="inputContainer optionalContainer">
                    <label class="optionalTitle" for="optional">Escolha os opcionais:</label>
                    <div class="checkboxContainer" v-for="optional in optionalsData" :key="optional.id">
                        <input type="checkbox" name="optional" v-model="optionals" :value="optional.type">
                        <span>{{ optional.type }}</span>
                    </div>
                </div>
                <div class="inputContainer">
                    <input type="submit" class="submitBtn" value="Criar meu burger!">
                </div>
            </form>
        </div>
    </div>
</template>
<script>
import Message from "./Message.vue";
export default {
    name: "BurgerForm",
    components: { Message },
    data() {
        return {
            breads: null,
            meats: null,
            optionalsData: null,
            name: null,
            bread: null,
            meat: null,
            optionals: [],
            msg: ""
        };
    },
    methods: {
        async getIngredients() {
            const req = await fetch("http://localhost:3000/ingredients");
            const data = await req.json();
            this.breads = data.breads;
            this.meats = data.meats;
            this.optionalsData = data.optionals;
        },
        async createBurger(e) {
            e.preventDefault();
            const data = {
                name: this.name,
                meat: this.meat,
                bread: this.bread,
                optionals: Array.from(this.optionals),
                status: "Solicitado"
            };
            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();
            // System Message

            this.msg = `Pedido N.º${res.id} realizado com sucesso`
            // Clear Message
            setTimeout(() =>
                this.msg = "", 3000)
            // Clear the form 
            this.name = "";
            this.meat = "";
            this.bread = "";
            this.optionals = [];
        }
    },
    mounted() {
        this.getIngredients();
    }
}

</script>
<style scoped>
.burgerForm {
    max-width: 400px;
    margin: 0 auto;
}

.inputContainer {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    ;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
}

input,
select {
    padding: 5px 10px;
    width: 300px;
}

.optionalContainer {
    flex-direction: row;
    flex-wrap: wrap;
}

.optionalTitle {
    width: 100%;
}

.checkboxContainer {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkboxContainer span,
.checkboxContainer input {
    width: auto;
}

.checkboxContainer span {
    margin-left: 6px;
    font-weight: bold;
}

.submitBtn {
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.submitBtn:hover {
    background-color: transparent;
    color: #222;
}
</style>