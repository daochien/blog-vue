<template>
    <div class="row mt-5">
        <div class="col-md-12">
        <div class="card">
            <div class="card-header">
                <h3 class="card-title">Users Table</h3>

                <div class="card-tools">
                    <button class="btn btn-success" @click="newModal()">Add New <i class="fas fa-user-plus"></i></button>
                </div>
            </div>
            <!-- /.card-header -->
            <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                    <tbody>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Email</th>
                            <th>Type</th>
                            <th>Registered At</th>
                            <th>Modify</th>
                        </tr>
                        <tr v-for="user in users" :key="user.id">
                            <td>{{user.id}}</td>
                            <td>{{user.name}}</td>
                            <td>{{user.email}}</td>
                            <td>{{user.type | upText }}</td>
                            <td>{{user.created_at | myDate }}</td>
                            <td>
                                <a href="#" @click="editModal(user)">
                                    <i class="fa fa-edit"></i>
                                </a>
                                /
                                <a href="#" @click="deleteUser(user.id)">
                                    <i class="fa fa-trash"></i>
                                </a>
                            </td>
                        </tr>

                    </tbody>
                </table>
            </div>
            <!-- /.card-body -->
        </div>
        <!-- /.card -->
        </div>
        <div class="modal fade" id="addNewUser" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" v-show="!editmode" id="exampleModalLabel">Add user</h5>
                    <h5 class="modal-title" v-show="editmode" id="exampleModalLabel">Update User's Infor</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <form @submit.prevent=" editmode ? updateUser() : createUser()">
                    <div class="modal-body">
                        <div class="form-group">
                            <input type="text" v-model="form.name" name="name" class="form-control" :class="{'is-invalid': form.errors.has('name') }" placeholder="Name" >
                            <has-error :form="form" field="name"></has-error>
                        </div>
                        <div class="form-group">
                            <input type="email" v-model="form.email" name="email" class="form-control" :class="{'is-invalid': form.errors.has('email') }" placeholder="Email Adress" >
                            <has-error :form="form" field="email"></has-error>
                        </div>
                        <div class="form-group">
                            <textarea v-model="form.bio" name="bio" id="bio" placeholder="Short bio for user (Optional)" class="form-control" :class="{'is-invalid': form.errors.has('bio') }"></textarea>
                            <has-error :form="form" field="bio"></has-error>
                        </div>
                        <div class="form-group">
                                <select name="type" v-model="form.type" id="type" class="form-control" :class="{'is-invalid': form.errors.has('type') }">
                                    <option value="">Select User Role</option>
                                    <option value="admin">Admin</option>
                                    <option value="user">Standard User</option>
                                    <option value="author">Author</option>
                                </select>
                                <has-error :form="form" field="type"></has-error>
                        </div>
                        <div class="form-group">
                            <input type="password" v-model="form.password" name="password" class="form-control" :class="{'is-invalid': form.errors.has('password') }" >
                            <has-error :form="form" field="password"></has-error>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                        <button v-show="!editmode" type="submit" class="btn btn-primary">Create</button>
                        <button v-show="editmode" type="submit" class="btn btn-primary">Update</button>
                    </div>
                </form>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            editmode: false,
            users: [],
            form: new Form({
                id: '',
                name: '',
                email: '',
                password: '',
                type: '',
                bio: '',
                photo: '',
            })
        };
    },
    methods:{
        editModal(user){
            this.editmode = true;
            this.form.reset();
            this.form.clear();
            this.form.fill(user);
            $("#addNewUser").modal('show');
        },
        newModal(){
            this.editmode = false;
            this.form.reset();
            this.form.clear();
            $("#addNewUser").modal('show');
        },
        createUser(){
            this.form.post('api/user').then( () => {
                Fire.$emit('AfterCreated');
                $("#addNewUser").modal('hide');
                toast({
                    type: 'success',
                    title: 'Signed in successfully'
                });
            }).catch( () => {

            });

        },
        deleteUser(id){
            swal({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                }).then((result) => {

                    if (result.value) {
                        this.form.delete('api/user/'+id).then( () => {
                            swal(
                                'Deleted!',
                                'Your file has been deleted.',
                                'success'
                            );
                            Fire.$emit('AfterCreated');
                        }).catch( () => {
                            swal('Deleted!', 'There was something wronge.', 'warring');
                        });

                    }
                });
        },
        loadUsers(){
            axios.get('api/user')
            .then(({ data }) => {
                this.users = data.data;
            })
            .catch(error => {
                console.log(error);
            });
        },

        updateUser(){
            this.form.put('api/user/'+this.form.id).then( (result) => {
                console.log(result);
                swal(
                    'Updated!',
                    'Information has been updated',
                    'success'
                );
                Fire.$emit('AfterCreated');
                $("#addNewUser").modal('hide');
            }).catch( (error) => {
                console.log(error);
            } );
        }

    },
    created(){
        this.loadUsers();
        Fire.$on('AfterCreated', () => {
            this.loadUsers();
        })
    }
}
</script>
<style lang="scss" scoped>

</style>


