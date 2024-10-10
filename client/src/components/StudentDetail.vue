<!-- <template>
  <div></div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";

const students = ref([]);

const newStudent = {
  student_name: "Nguyen Van D",
  email: "nguyenvand@example.com",
  address: "123 Nguyen Trai, Hanoi",
  phone: "0912345678",
  status: true,
  created_at: "2024-10-01T09:00:00Z",
};

const updatedStudent = {
  student_name: "Nguyen Van H",
  email: "nguyenvanH@example.com",
  address: "123 Nguyen Trai, Hanoi",
  phone: "0912345678",
  status: true,
  created_at: "2024-10-01T09:00:00Z",
};

const getStudentById = async () => {
  try {
    const response = await axios.get("http://localhost:8080/students/2");
    if (response.data) {
      console.log(response.data);
    } else {
      console.log("không tìm thấy bản ghi");
    }
  } catch (error) {
    console.error("Lỗi khi lấy dữ liệu", error);
  }
};

const removeById = async (id) => {
  try {
    const response = await axios.delete(`http://localhost:8080/students/${id}`);
    if (response.status === 200) {
      console.log(`Đã xóa sinh viên với ID: ${id}`);
    } else {
      console.log("Không thể xóa sinh viên");
    }
  } catch (error) {
    console.error("Lỗi khi xóa sinh viên", error);
  }
};

const createStudent = async (student) => {
  try {
    const response = await axios.post(
      "http://localhost:8080/students",
      student
    );
    if (response.status === 201) {
      console.log("Sinh viên mới đã được thêm:", response.data);
    } else {
      console.log("Không thể thêm sinh viên mới");
    }
  } catch (error) {
    console.error("Lỗi khi thêm sinh viên mới", error);
  }
};

const updateStudentById = async (id, student) => {
  try {
    const response = await axios.put(
      `http://localhost:8080/students/${id}`,
      student
    );
    if (response.status === 200) {
      console.log(
        `Đã cập nhật thông tin sinh viên với ID: ${id}`,
        response.data
      );
    } else {
      console.log("Không thể cập nhật sinh viên");
    }
  } catch (error) {
    console.error("Lỗi khi cập nhật sinh viên", error);
  }
};

onMounted(() => {
  getStudentById();
  removeById(6);
  createStudent(newStudent);
  updateStudentById(2, updatedStudent);
});
</script>

<style></style> -->
