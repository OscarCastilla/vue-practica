<template>
    <h1>Aqui podras editar un curso</h1>
        
        <form @submit.prevent="updateCourse">
            <div>
                <label for="title">Titulo</label><br>
                <input type="text" id="title" v-model="course.title" placeholder="Ingresa el titulo del curso">
            </div>
            <br>
            <div>
                <label for="description">Descripcion</label><br>
                <textarea id="description" v-model="course.description" placeholder="Ingresa la descripcion del curso" ></textarea>
            </div>

            <br>

            <div>
                <label for="category">Categoría</label><br>
                <select name="" id="category" v-model="course.category_id" >
                    <option value="" selected disabled>Seleccione una categoria</option>
                    <option v-for="category in categories" :key="'category-'+category.id" :value="category.id">
                        {{category.name}}
                    </option>
                </select>
            </div>
            <br>
            
            <button type="submit">Guardar</button>

        </form>


</template>

<script>
export default {
    data(){
        return{

            course:{},
            categories:[],

        }
    },

    created(){
        this.getCourse();
        this.getCategories();
    },

    methods:{

         getCourse(){
            this.axios.get("https://cursos-prueba.tk/api/courses/"+this.$route.params.id+ '?included=category')
            .then((response)=>{
                this.course=response.data;

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

        updateCourse(){
            this.axios.put('https://cursos-prueba.tk/api/courses/'+this.$route.params.id,this.course)
                .then(()=>{
                    this.$router.push({name:'courseDetails',params:{id:this.$route.params.id}});
                })
                .catch((error)=>{
                    console.log(error);
                });
        }

    }
}
</script>
  


<style>

</style>