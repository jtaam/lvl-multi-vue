<style>
  .widget-user-header{
    background-position: center center;
    background-size: cover;
    height: 250px !important;
  }
</style>
<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-3">

              <div class="card card-widget widget-user">
                <!-- Add the bg color to the header using any of the bg-* classes -->
                <div class="widget-user-header text-white" style="background: url('./img/user-cover.png') center center;">
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




                    </div>
                    <!-- /.tab-pane -->

                    <div class="tab-pane active show" id="settings">
                      <form class="form-horizontal">
                        <div class="form-group">
                          <label for="inputName" class="col-sm-2 control-label">Name</label>

                          <div class="col-sm-10">
                            <input v-model="form.name" type="email" class="form-control" id="inputName" placeholder="Name">
                          </div>
                        </div>
                        <div class="form-group">
                          <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                          <div class="col-sm-10">
                            <input v-model="form.email" type="email" class="form-control" id="inputEmail" placeholder="Email">
                          </div>
                        </div>

                        <div class="form-group">
                          <label for="inputBio" class="col-sm-2 control-label">Bio</label>

                          <div class="col-sm-10">
                            <textarea v-model="form.bio" class="form-control" id="inputBio" placeholder="Bio"></textarea>
                          </div>
                        </div>

                        <div class="form-group">
                          <label for="profilePhoto" class="col-sm-2 control-label">Profile Photo</label>

                          <div class="col-sm-10">
                            <input type="file" @change="updateProfile" name="profilePhoto" id="profilePhoto" class="form-input">
                          </div>
                        </div>


                        <div class="form-group">
                          <label for="password" class="col-sm-12 control-label">Password (leave empty if not changing)</label>

                          <div class="col-sm-10">
                            <input type="password" v-model="form.password" class="form-control" id="password" placeholder="Password" autocomplete="off">
                          </div>
                        </div>

                        <div class="form-group">
                          <div class="col-sm-offset-2 col-sm-10">
                            <button type="submit" @click.prevent="updateInfo" class="btn btn-success">Update</button>
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
        return{
          form: new Form({
            id:'',
            name:'',
            email:'',
            password:'',
            type:'',
            bio:'',
            photo:''
          })
        }
      },
      methods:{
        updateProfile(e){
          let file = e.target.files[0];

          console.log(file);

          let reader = new FileReader();
          if (file['size'] < 2111775) {
          // if (file['size'] > 703925) {
            reader.onloadend = (file) => {
              // console.log('RESULT', reader.result);
              this.form.photo = reader.result;
            }
            reader.readAsDataURL(file);
          }else{
            swal({
              type:'error',
              title:'Oops...',
              text:'You are uploading a large file.',
            })
          }
        },
        updateInfo(){
          this.$Progress.start();
          this.form.put('api/profile/')
            .then(()=>{

              this.$Progress.finish();
            })
            .catch(()=>{
              this.$Progress.fail();
            });
        }
      },
      mounted() {
          console.log('Component mounted.')
      },
      created(){
        axios.get('api/profile')
          .then(({ data }) => (this.form.fill(data)));
      }
    }
</script>
