<template>
    <div class='container py-4'>
        <div class='row justify-content-center'>
            <div class='col-md-8'>
            <div class='card'>
                <div class='card-header'>Semua Kontak</div>
                <div class='card-body'>
                    <router-link :to="{ name: 'create' }" class="btn btn-primary">Tambah Kontak</router-link>
                    <br/>
                    <br/>
                    <div class="table-responsive">
                        <table class="table table-bordered table-hover">
                            <thead>
                                <tr>
                                    <th width="50" class="text-center">No</th>
                                    <th>Title</th>
                                    <th>Number</th>
                                    <th width="200" class="text-center">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(contact, index) in contacts" :key="contact.id">
                                    <td width="50" class="text-center">{{ index + 1 }}</td>
                                    <td>{{ contact.name }}</td>
                                    <td>{{ contact.number }}</td>
                                    <td width="200" class="text-center">
                                        <div class="btn-group">
                                            <router-link :to="{name: 'show', params: { id: contact.id }}" class="btn btn-primary">Detail</router-link>
                                            <router-link :to="{name: 'edit', params: { id: contact.id }}" class="btn btn-success">Edit</router-link>
                                            <button class="btn btn-danger" @click = "deletePost(contact.id)">Delete</button>
                                        </div>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            </div>
        </div>
        </div>
</template>

<script>
  export default {
      data() {
        return {
          contacts: []
        }
      },
      created() {
        let uri = 'http://localhost:8000/api/contacts';
        this.axios.get(uri).then(response => {
            this.contacts = response.data.data;
        });
    },
    methods: {
        deletePost(id)
        {
            this.$swal.fire({
                title: 'Apakah kamu yakin?',
                text: "Jika kamu hapus, maka data tidak akan kembali lagi.",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Hapus Deh',
                cancelButtonText: 'Nggak Jadi'
                }).then((result) => {
                if (result.value) {
                    this.$swal.fire({
                        title: 'Success!',
                        text: 'Article deleted successfully',
                        icon: 'success',
                        timer: 1000
                    });
                    let uri = `http://localhost:8000/api/contact/delete/${id}`;
                    this.axios.delete(uri).then(response => {
                        this.contacts.splice(this.contacts.indexOf(id), 1);
                    });
                    console.log("Deleted article with id ..." +id);
                }
            })
        }
    }
  }
</script>
