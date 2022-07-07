<template>
    <h1>Listado de cursos</h1>


        <ul v-if="errors.length>0">
            <li v-for="error in errors" :key="error.id">
                        {{error}}
            </li>
        </ul>


        <form @submit.prevent="saveCourse" class="mb-4">
            <div class="mb-2">
                <label for="title">Titulo</label><br>
                <input type="text" id="title" v-model="course.title" placeholder="Ingresa el titulo del curso">
            </div>
      
            <div class="mb-2">
                <label for="description">Descripcion</label><br>
                <textarea id="description" v-model="course.description" placeholder="Ingresa la descripcion del curso" ></textarea>
            </div>


            <div class="mb-2">
                <label for="category">Categoría</label><br>
                <select name="" id="category" v-model="course.category_id" >
                    <option value="" selected disabled>Seleccione una categoria</option>
                    <option v-for="category in categories" :key="'category-'+category.id" :value="category.id">
                        {{category.name}}
                    </option>
                </select>
            </div>
            
            <button type="submit" class="btn btn-primary btn-sm mb-2">Guardar</button>

        </form>
        <ul>
            <li v-for="course in courses" :key="'coruse-'+course.id" class="mb-2">
                <router-link :to="{name:'courseDetails', params:{id:course.id}}">
                        {{course.title}}
                </router-link>
                -
                <button @click="deleteCourse(course.id)" class="btn btn-danger btn-sm">Eliminar</button>

            </li>
        </ul>

        <!-- paginacion -->

        <div class="d-flex justify-content-center">

             <nav aria-label="...">
                 <ul class="pagination">
                    <li v-for="pagination_link in pagination_liks"
                    :key="'pagination_link-'+pagination_link.label"
                     class="page-item "
                     :class="{
                        'disabled':pagination_link.url==null,
                        'active':pagination_link.active
                     }">
                    <a class="page-link"
                    @click="changuePague(pagination_link.url)"
                    v-html="pagination_link.label" style="cursor:pointer">
                   
                    </a>
                    </li>
                </ul>
        </nav>


     
            
        </div>
    

</template>

<script>
export default {
    data(){
        return{

            categories:[],
            courses:[],
            course:{
                title:'',
                description:'',
                category_id:''

            },
            pagination_liks:[],
            errors:[],
        }
    },

    created(){
        this.getCourses();
        this.getCategories();
    },

    computed:{
        page(){
            return this.$route.query.page ?? 1;
        }
        
    },

    watch:{
        page(){
            this.getCourses()
        }
    },


    methods:{
        getCourses(){
            this.axios.get("https://cursos-prueba.tk/api/courses?per_page=10"+ '&page='+this.page)
            .then((response)=>{
                let res=response.data;
                this.courses=res.data;
                this.pagination_liks=res.links;

            }).catch(error=>{
                console.log(error);
            });
        },

        getCategories(){

             this.axios.get("https://cursos-prueba.tk/api/categories")
            .then((response)=>{
                this.categories=response.data;

            }).catch(error=>{
                console.log(error);
            });

        },

        saveCourse(){
           
            this.axios.post('https://cursos-prueba.tk/api/courses',this.course)
            .then((response)=>{

                let course=response.data;
                this.courses.push(course);
                this.course={
                     title:'',
                    description:'',
                    category_id:''
                }

                this.errors=[];

            }).catch((error)=>{
                this.errors=Object.values(error.response.data.errors).flat();
            });


        },

        deleteCourse(id){
            this.axios.delete('https://cursos-prueba.tk/api/courses/'+id)
                .then(()=>{
                    this.courses=this.courses.filter(course=>course.id !=id);
                }).catch((error)=>{
                    console.log(error);
                });
        },

        changuePague(url){
            this.$router.replace({
                query:{
                    page:url.split('page=')[1]
                }
            });

        },

        
    }

}
</script>

<style>

</style>