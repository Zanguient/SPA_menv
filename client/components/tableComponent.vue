<template>

    <b-row>
        <table class="table table-bordered table-hover">
            <thead class="thead-light">
            <tr>
                <th>Name</th>
                <th>Email</th>
                <th>Phone</th>
                <th>Providers</th>
                <th> </th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="client in clients" :key="client._id">
                <td>{{client.name}}</td>
                <td>{{client.email}}</td>
                <td>{{client.phone}}</td>
                <td>{{client.providers.map(function (provider) {
                    return (provider.name);
                    }).sort().join(', ')}}</td>

                <!--button edit client-->
                <td><edit-client-component :id="client._id"></edit-client-component></td>
            </tr>
            </tbody>
        </table>
    </b-row>

</template>

<script>
    import ClientService from '../services/ClientService.js';
    import EditClientComponent from "./editClientComponent.vue";
    export default {
        name: 'tableComponent',
        components: {
            EditClientComponent,
        },
        data() {
            return {
                clients: [],
            };
        },
        methods: {
            async getClients() {
                this.clients = await ClientService.fetchClients();
            }
        },
        created() {
            this.getClients();
        },
        mounted() {
            this.$root.$on('updateClients', () => {
                this.getClients();
            });
        }
    }

</script>