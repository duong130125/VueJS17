<!-- <template>
  <div></div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";

const students = ref([]);

const getAllStudent = async () => {
  try {
    const response = await axios.get("http://localhost:8080/students");
    students.value = response.data;
    console.log(response.data);
  } catch (error) {
    console.error("Lỗi khi lấy dữ liệu", error);
  }
};

onMounted(() => {
  getAllStudent();
});
</script> -->
