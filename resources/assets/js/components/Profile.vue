<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-md-12">
                <div class="card card-widget widget-user">
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header text-white" style="background-image: url('./img/user-cover.jpg');">
                        <h3 class="widget-user-username">Elizabeth Pierce</h3>
                        <h5 class="widget-user-desc">Web Designer</h5>
                    </div>
                    <div class="widget-user-image">
                        <img class="img-circle" src="" alt="User Avatar">
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
                  <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                  <li class="nav-item"><a class="nav-link active show" href="#settings" data-toggle="tab">Settings</a></li>
                </ul>
              </div><!-- /.card-header -->
              <div class="card-body">
                    <div class="tab-content">
                        <div class="tab-pane" id="activity">
                            <h3>Display user activity</h3>
                        </div>
                        <!-- /.tab-pane -->


                        <div class="tab-pane active show" id="settings">
                            <form class="form-horizontal">
                                <div class="form-group">
                                    <label for="name" class="col-sm-2 control-label">Name</label>

                                    <div class="col-sm-10">
                                        <input type="text" v-model="form.name" class="form-control" id="name" placeholder="Name">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label for="email" class="col-sm-2 control-label">Email</label>

                                    <div class="col-sm-10">
                                    <input type="email" v-model="form.email" class="form-control" id="email" placeholder="Email">
                                    </div>
                                </div>                                
                                <div class="form-group">
                                    <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                                    <div class="col-sm-10">
                                    <textarea class="form-control" id="inputExperience" placeholder="Experience"></textarea>
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label for="profile_photo" class="col-sm-2 control-label">Profile Photo</label>
                                    <div class="col-sm-10">
                                        <input type="file" id="profile_photo" >
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label for="inputSkills" class="col-sm-2 control-label">Passport</label>

                                    <div class="col-sm-10">
                                    <input type="text" class="form-control" id="inputSkills" placeholder="Passport">
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
        data(){
            return {                
                form: new Form({
                    id: '',
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: '',
                })
            }
        },
        created(){
            axios.get('api/profile').then( (response) => {
                this.form.fill(response.data);
            }).catch( (error) => {
                console.log(error);
            });
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
<style lang="scss" scoped>
    .widget-user-header{
        background-position: center center;
        background-size: cover;
        height: 300px;
    }
</style>

