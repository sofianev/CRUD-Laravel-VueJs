<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-6">
                <div class="card">
                    <div class="card-header">Create new Student</div>

                    <div class="card-body">
                        <form>
                             <div class="form-group">
                                <label for="exampleInputPassword1"
                                    >name</label
                                >
                                <input
                                    type="text"
                                    v-model="name"
                                    class="form-control"
                                    id="name"
                                    placeholder="name"
                                />
                            </div>
                            <div class="form-group">
                                <label for="exampleInputEmail1"
                                    >Email address</label >
                                <input
                                    type="email"
                                     v-model="email"
                                    class="form-control"
                                    id="exampleInputEmail1"
                                    aria-describedby="emailHelp"
                                    placeholder="Enter email" />
                                
                            
                            </div>
                             <div class="form-group">
                                <label for="exampleInputEmail1"
                                    >phonee</label >
                                <input
                                    type="phone"
                                     v-model="phone"
                                    class="form-control"
                                    id="phone"
                                   
                                    placeholder="phone" />
                                
                            
                            </div>

                           
                            <div class="form-check">
                                <input
                                    type="checkbox"
                                    class="form-check-input"
                                    id="exampleCheck1"
                                />
                                <label
                                    class="form-check-label"
                                    for="exampleCheck1"
                                    >Check me out</label
                                >
                            </div>
                            <button type="submit" @click.prevent="savestudent" class="btn btn-primary">
                                Submite
                            </button>
                        </form>
                    </div>
                </div>
               
            </div>
 <div class="col-md-6">
 <div class="card">
 <div class="card-header">all students </div>
 <div class="card-body">
 <table class="table table-dark">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">nameee</th>
      <th scope="col">email</th>
      <th scope="col">phone</th>
      <th scope="col">Action</th>
      
    </tr>
  </thead>
  <tbody>
    <tr v-for="(student,index) in students.data" :key="student.id">
      <th scope="col">{{index + 1}}</th>
      <th scope="col">{{student.name}}</th>
      <th scope="col">{{student.email}}</th>
      <th scope="col">{{student.phone}}</th>
       <th scope="col">
           <!-- Button trigger modal -->
               <button type="button" @click="editstudent(student.id)" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
            Edit
                </button>
                <!-- Button trigger modal -->
           <button type="button" @click="deletestudent(student.id)" data-dismiss="modal" class="btn btn-danger" >
            Delete
           </button>
 
        </th>
    </tr>
      
  </tbody>
</table>
<pagination :data="students" @pagination-change-page="getResults"></pagination>
     </div>
     </div>
    </div>
     </div>

               <!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
       <form>
                             <div class="form-group">
                                <label for="exampleInputPassword1"
                                    >name</label
                                >
                                <input
                                    type="text"
                                    v-model="editname"
                                    class="form-control"
                                    id="name"
                                    placeholder="name"
                                />
                            </div>
                            <div class="form-group">
                                <label for="exampleInputEmail1"
                                    >Email address</label >
                                <input
                                    type="email"
                                     v-model="editemail"
                                    class="form-control"
                                    id="exampleInputEmail1"
                                    aria-describedby="emailHelp"
                                    placeholder="Enter email" />
                                
                            
                            </div>
                             <div class="form-group">
                                <label for="exampleInputEmail1"
                                    >phone</label >
                                <input
                                    type="phone"
                                     v-model="editphone"
                                    class="form-control"
                                    id="phone"
                                   
                                    placeholder="phone" />
                                
                            
                            </div>

                           
                            <div class="form-check">
                                <input
                                    type="checkbox"
                                    class="form-check-input"
                                    id="exampleCheck1"
                                />
                                <label
                                    class="form-check-label"
                                    for="exampleCheck1"
                                     >Check me out</label
                                >
                            </div>
                           
                            
                        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" @click.prevent="updatestudent" data-dismiss="modal" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
       
    
     </div>
</template>

<script>
export default {
    data(){
        return{
        students:{} ,
        id:'',
         name:'',
         email:'',
         phone:'' ,
         editname:'',
        editemail:'',
        editphone:''  }
    },
    mounted() {
       this.getResults();
       
    },
    methods:{
        savestudent(){
        axios.post('save_student',{
         name: this.name,
         email:this.email,
         phone:this.phone
       })
        .then(response=>{
            this.name='',
            this.email='',
            this.phone= '',
             this.getResults();
           });
    },
    getResults(page = 1) {
			axios.get('student_all?page=' + page)
				.then(response => {
                    console.log(response.data);
					this.students = response.data;
				});
		},
        editstudent(id){
          axios.get('edit_student/'+id)
          .then(response => {
            
             this.id=response.data.id;
             this.editname=response.data.name;
             this.editemail=response.data.email;
             this.editphone=response.data.phone;

              });
        },

        updatestudent()  { 
        
          axios.put('update_student', {

                 id:  this.id,
                name: this.editname,
                email:this.editemail,
                phone:this.editphone,
            })
              .then(response=>{
                   this.getResults();
                   });


              
        },
        
        deletestudent(id){

            axios.delete('delete_student/'+id) 

              .then(response=> {this.getResults();});
                   
        } 
    }
}
</script>
