<template>
    <div class="container">
      <div class="row mt-5">
        <div class="col-md-12">
          <div class="card">
            <div class="card-header">
              <h3 class="card-title">Users Details</h3>

              <div class="card-tools">
                <button class="btn btn-success">Add User <i class="fas fa-user-plus"></i></button>
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
                    <th>Modify</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>1</td>
                    <td>Dinesh Babu</td>
                    <td>dinesh@nanocorporation.co.uk</td>
                    <td>Admin</td>
                    <td>
                      <a href="#" class="mr-1"><i class="fas fa-edit blue"></i></a>
                      <a href="#" class="mr-1"><i class="fas fa-trash-alt red"></i></a>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
