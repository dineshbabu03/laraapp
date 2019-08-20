<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12 mt-3">
              <div class="card card-widget widget-user">
                  <!-- Add the bg color to the header using any of the bg-* classes -->
                  <div class="widget-user-header text-white" style="background: url('/image/photo1.png') center center;">
                    <h3 class="widget-user-username">Elizabeth Pierce</h3>
                    <h5 class="widget-user-desc">Web Designer</h5>
                  </div>
                  <div class="widget-user-image">
                    <img class="img-circle" src="/image/user.png" alt="User Avatar">
                  </div>
                  <div class="card-footer">
                    <div class="row">
                      <div class="col-sm-4 border-right">
                        <div class="description-block">
                          <h5 class="description-header">3,200</h5>
                          <span class="description-text">SALES</span>
                        </div>
                        <!-- /.description-block -->
                      </div>
                      <!-- /.col -->
                      <div class="col-sm-4 border-right">
                        <div class="description-block">
                          <h5 class="description-header">13,000</h5>
                          <span class="description-text">FOLLOWERS</span>
                        </div>
                        <!-- /.description-block -->
                      </div>
                      <!-- /.col -->
                      <div class="col-sm-4">
                        <div class="description-block">
                          <h5 class="description-header">35</h5>
                          <span class="description-text">PRODUCTS</span>
                        </div>
                        <!-- /.description-block -->
                      </div>
                      <!-- /.col -->
                    </div>
                    <!-- /.row -->
                  </div>
                </div>
            </div>


            <div class="col-md-12">
            <div class="card">
              <div class="card-header p-2">
                <ul class="nav nav-pills">
                  <li class="nav-item"><a class="nav-link active" href="#activity" data-toggle="tab">Activity</a></li>
                  <li class="nav-item"><a class="nav-link" href="#settings" data-toggle="tab">Settings</a></li>
                </ul>
              </div>
              <div class="card-body">
                <div class="tab-content">
                  <div class="active tab-pane" id="activity">
                      <h2>Display User Activity</h2>
                  </div>

                  <div class="tab-pane" id="settings">
                    <form class="form-horizontal">
                      <div class="form-group">
                        <label for="inputName" class="col-sm-10 control-label">Name</label>

                        <div class="col-sm-10">
                          <input v-model="form.name" type="email" class="form-control" id="name" placeholder="Name">
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="inputEmail" class="col-sm-10 control-label">Email</label>

                        <div class="col-sm-10">
                          <input v-model="form.email" type="email" class="form-control" id="email" placeholder="Email">
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="inputPasswprd" class="col-sm-10 control-label">Password (leave empty if not changing)</label>

                        <div class="col-sm-10">
                          <input v-model="form.password" type="password" class="form-control" id="password" placeholder="Password">
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="inputName2" class="col-sm-10 control-label">Type</label>

                        <div class="col-sm-10">
                          <input v-model="form.type" type="text" class="form-control" id="type" placeholder="Type">
                        </div>
                      </div>
                      <div class="form-group">
                        <label for="inputExperience" class="col-sm-10 control-label">About Me</label>

                        <div class="col-sm-10">
                          <textarea v-model="form.bio" class="form-control" id="bio" placeholder="About Me"></textarea>
                        </div>
                      </div>
                      <div class="form-group">
                        <div class="col-sm-offset-2 col-sm-10">
                          <div class="checkbox">
                            <label>
                              <input type="checkbox"> I agree to the <a href="#">terms and conditions</a>
                            </label>
                          </div>
                        </div>
                      </div>
                      <div class="form-group">
                        <div class="col-sm-offset-2 col-sm-10">
                          <button type="submit" class="btn btn-danger">Submit</button>
                        </div>
                      </div>
                    </form>
                  </div>
                  <!-- /.tab-pane -->
                </div>
                <!-- /.tab-content -->
              </div><!-- /.card-body -->
            </div>
            <!-- /.nav-tabs-custom -->
          </div>



        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                users: {},

                form: new Form ({
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
            showUser() {
                axios
                  .get("api/profile")
                  .then(({ data }) => (this.form.fill(data)));
            },
        },

        created() {
            this.showUser();
        },

        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
