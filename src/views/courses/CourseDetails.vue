<template>
    <h1>{{course.title}}</h1>
    <p>{{course.description}}</p>
    <p>
        <b>Categoria :</b>
        {{course.category.name}}
    </p>


    <router-link :to="{name:'courseEdit',params:{id:course.id}}">
            Editar Curso
    </router-link>
   
</template>

<script>
export default {
    data(){
        return{
            course:{}
        }
    },

    created(){
        this.getCourse();
    },

    methods: {
          getCourse(){
            this.axios.get("https://cursos-prueba.tk/api/courses/"+this.$route.params.id+ '?included=category')
            .then((response)=>{
                this.course=response.data;

            }).catch(error=>{
                console.log(error);
            });
        }
    }




}
</script>

<style>

</style>