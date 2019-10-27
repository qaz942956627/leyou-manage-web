<template>
  <v-card>
      <v-flex xs12 sm10>
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>

  export default {
    name: "category",
    data() {
      return {
        isEdit:true,
        //treeData
      }
    },
    methods: {
      handleAdd(node) {
          console.log("add .... ");
          console.log(node);
          if (node.parentId !== 0) {
              this.$http({
                  method: 'post',
                  url: '/item/category',
                  data: this.$qs.stringify(node)
              }).then(() => {
                  this.reloadData(node.id);
              }).catch();
          }else {
              this.$message.error("刷新后重试！");
          }
      },
      handleEdit(id, name) {
          console.log("edit... id: " + id + ", name: " + name);
          const node={
              id:id,
              name:name
          };
          if (node.parentId !== 0) {
              this.$http({
                  method: 'put',
                  url: '/item/category',
                  data: this.$qs.stringify(node)
              }).then(() => {
                  this.$message.info("修改成功！");
              }).catch(() => {
                  this.$message.info("修改失败！");
              });
          }else {
              this.$message.error("刷新后重试！");
          }
      },
      handleDelete(id,parentId) {
          console.log("delete ... " + id);
          if (parentId !== 0) {
              this.$http({
                  method: 'delete',
                  url: '/item/category/'+id
              }).then(() => {
                  this.$message.info("删除成功！");
              }).catch(() => {
                  this.$message.info("删除失败！");
              })
          }else {
              this.$message.error("刷新后重试！");
          }
      },
      handleClick(node) {
        console.log(node)
      },
        reloadData(id){
            //操作完成后刷新数据
            this.$http.get("/item/category/list?pid="+id).then().catch();
        }
    }
  };
</script>

<style scoped>

</style>
