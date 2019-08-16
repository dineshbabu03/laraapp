<template>
    <div class="container">
      <div class="row">
        <div class="col-md-12 mt-3">
          <div class="card">
            <div class="card-header">
              <h3 class="card-title">Users Details</h3>

              <div class="card-tools">
                <button
                  class="btn btn-success"
                  @click="newModal">
                    Add User <i class="fas fa-user-plus"></i>
                </button>
              </div>
            </div>

            <div class="card-body table-responsive p-0">
              <table class="table table-hover">
                <thead>
                  <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Type</th>
                    <th>Registered</th>
                    <th>Modify</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="user in users">
                    <td>{{ user.id }}</td>
                    <td>{{ user.name }}</td>
                    <td>{{ user.email }}</td>
                    <td>{{ user.type | upText }}</td>
                    <td>{{ user.created_at | myDate }}</td>
                    <td>
                      <a href="#" class="mr-1" @click="editModal(user)"><i class="fas fa-edit blue"></i></a>
                      <a href="#" class="mr-1" @click="deleteUser(user.id)"><i class="fas fa-trash-alt red"></i></a>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>

      <!-- Modal -->
      <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel" v-show="!editMode">Add New User</h5>
              <h5 class="modal-title" id="exampleModalLabel" v-show="editMode">Update User</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>

            <form @submit.prevent="editMode ? updateUser() : createUser()">
              <div class="modal-body">
                <div class="form-group">
                  <input v-model="form.name" type="text" name="name" placeholder="Name"
                    class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                  <has-error :form="form" field="name"></has-error>
                </div>

                <div class="form-group">
                  <input v-model="form.email" type="text" name="email" placeholder="Email"
                    class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                  <has-error :form="form" field="email"></has-error>
                </div>

                <div class="form-group">
                  <input v-model="form.password" type="password" name="password" placeholder="Password"
                    class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                  <has-error :form="form" field="password"></has-error>
                </div>

                <div class="form-group">
                  <select v-model="form.type" name="type" class="form-control" id="type" :class="{ 'is-invalid': form.errors.has('type') }">
                    <option value="">Select User Role</option>
                    <option value="admin">Admin</option>
                    <option value="user">User</option>
                    <option value="author">Author</option>
                  </select>
                  <has-error :form="form" field="type"></has-error>
                </div>

                <div class="form-group">
                  <textarea v-model="form.bio" name="bio" class="form-control" id="bio" rows="3" placeholder="Additional user details (optional)">
                  </textarea>
                </div>

              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                <button type="submit" class="btn btn-success" v-show="!editMode">Create</button>
                <button type="submit" class="btn btn-primary" v-show="editMode">Update</button>
              </div>
            </form>

          </div>
        </div>
      </div>

    </div>
</template>

<script>
    export default {

        data() {
            return {
              editMode: false,
              users: {},

              form: new Form({
                id: '',
                name: '',
                email: '',
                password: '',
                type: '',
                bio: '',
                photo: ''
              })
            }
        },

        methods: {

          displayUsers() {
              axios
                .get("api/user")
                .then(({ data }) => (this.users = data.data));
          },

          createUser() {
            this.$Progress.start()
            this.form.post('api/user')
              .then(() => {
                  Fire.$emit('AfterUserCreated')
                  $('#addNew').modal('hide')
                  Toast.fire({
                    type: 'success',
                    title: 'User added successfully'
                  })
                  this.$Progress.finish()
              })
              .catch(() => {
                  this.$Progress.fail()
              })
          },

          deleteUser(userid) {
            Swal.fire({
              title: 'Are you sure?',
              text: "You won't be able to revert this!",
              type: 'warning',
              showCancelButton: true,
              confirmButtonColor: '#3085d6',
              cancelButtonColor: '#d33',
              confirmButtonText: 'Yes, delete it!'
              }).then((result) => {
                if (result.value) {
                  //call api to delete user
                  this.form.delete('api/user/'+userid)
                    .then(() => {
                        Swal.fire(
                          'Deleted!',
                          'Your file has been deleted.',
                          'success'
                        )
                        Fire.$emit('AfterUserDeleted')
                    })
                    .catch(() => {
                      swal('Failed!', 'There was something wrong', 'warning');
                    });
                }
              })
          },

          updateUser() {
              this.form.put('api/user/'+this.form.id)
              .then(() => {
                  Fire.$emit('AfterUserUpdated')
                  $('#addNew').modal('hide')
                  Toast.fire({
                    type: 'success',
                    title: 'User Updated successfully'
                  })
                  this.$Progress.finish()
              })
              .catch(() => {
                  this.$Progress.fail()
              })
          },

          newModal() {
            this.editMode = false;
            this.form.clear()
            this.form.reset()
            $('#addNew').modal('show')
          },

          editModal(user) {
            this.editMode = true;
            this.form.clear()
            this.form.fill(user)
            $('#addNew').modal('show')
          }

        },

        created() {
            this.displayUsers();

            Fire.$on('AfterUserCreated', () => {
                this.displayUsers();
            });

            Fire.$on('AfterUserDeleted', () => {
                this.displayUsers();
            });

            Fire.$on('AfterUserUpdated', () => {
                this.displayUsers();
            })

            // setInterval(() => this.displayUsers(), 3000);
        }
    }
</script>
