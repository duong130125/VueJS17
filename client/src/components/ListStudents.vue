<template>
  <div>
    <div class="container-xl">
      <div class="table-responsive">
        <div class="table-wrapper">
          <div class="table-title">
            <div class="row">
              <div class="col-sm-6">
                <h2>Quản lý <b>sinh viên</b></h2>
              </div>
              <div class="col-sm-6">
                <a
                  href="#addEmployeeModal"
                  class="btn btn-success"
                  data-toggle="modal"
                  @click="openAddModal"
                >
                  <i class="material-icons">&#xE147;</i>
                  <span>Thêm mới sinh viên</span>
                </a>
              </div>
            </div>
          </div>
          <table class="table table-striped table-hover">
            <thead>
              <tr>
                <th>
                  <span class="custom-checkbox">
                    <input type="checkbox" id="selectAll" />
                    <label for="selectAll"></label>
                  </span>
                </th>
                <th>Tên sinh viên</th>
                <th>Email</th>
                <th>Địa chỉ</th>
                <th>Số điện thoại</th>
                <th>Lựa chọn</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="student in paginatedStudents" :key="student.id">
                <td>
                  <span class="custom-checkbox">
                    <input
                      type="checkbox"
                      :id="'checkbox' + student.id"
                      name="options[]"
                      :value="student.id"
                    />
                    <label :for="'checkbox' + student.id"></label>
                  </span>
                </td>
                <td>{{ student.student_name }}</td>
                <td>{{ student.email }}</td>
                <td>{{ student.address }}</td>
                <td>{{ student.phone }}</td>
                <td>
                  <a
                    href="#"
                    class="edit"
                    @click.prevent="openEditModal(student)"
                  >
                    <i class="material-icons" data-toggle="tooltip" title="Edit"
                      >&#xE254;</i
                    >
                  </a>
                  <a
                    href="#deleteEmployeeModal"
                    class="delete"
                    data-toggle="modal"
                    @click="openDeleteModal(student.id)"
                  >
                    <i
                      class="material-icons"
                      data-toggle="tooltip"
                      title="Delete"
                      >&#xE872;</i
                    >
                  </a>
                </td>
              </tr>
            </tbody>
          </table>
          <!-- Phân Trang -->
          <div class="clearfix">
            <div class="hint-text">
              Hiển thị <b>{{ paginatedStudents.length }}</b> /
              <b>{{ pageSize }}</b> bản ghi
            </div>
            <ul class="pagination">
              <li :class="{ 'page-item': true, disabled: currentPage === 1 }">
                <a href="#" @click.prevent="changePage(currentPage - 1)"
                  >Trước</a
                >
              </li>
              <li
                v-for="page in totalPages"
                :key="page"
                :class="{ 'page-item': true, active: page === currentPage }"
              >
                <a
                  href="#"
                  class="page-link"
                  @click.prevent="changePage(page)"
                  >{{ page }}</a
                >
              </li>
              <li
                :class="{
                  'page-item': true,
                  disabled: currentPage === totalPages,
                }"
              >
                <a href="#" @click.prevent="changePage(currentPage + 1)">Sau</a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <!-- Add Student Modal HTML -->
    <div id="addEmployeeModal" class="modal fade" ref="addModal">
      <div class="modal-dialog">
        <div class="modal-content">
          <form @submit.prevent="addStudent">
            <div class="modal-header">
              <h4 class="modal-title">Thêm mới sinh viên</h4>
              <button
                type="button"
                class="close"
                data-dismiss="modal"
                aria-hidden="true"
              >
                &times;
              </button>
            </div>
            <div class="modal-body">
              <div class="form-group">
                <label>Tên sinh viên</label>
                <input
                  v-model="newStudent.student_name"
                  type="text"
                  class="form-control"
                  required
                />
              </div>
              <div class="form-group">
                <label>Email</label>
                <input
                  v-model="newStudent.email"
                  type="email"
                  class="form-control"
                  required
                />
              </div>
              <div class="form-group">
                <label>Địa chỉ</label>
                <textarea
                  v-model="newStudent.address"
                  class="form-control"
                  required
                ></textarea>
              </div>
              <div class="form-group">
                <label>Số điện thoại</label>
                <input
                  v-model="newStudent.phone"
                  type="text"
                  class="form-control"
                  required
                />
              </div>
            </div>
            <div class="modal-footer">
              <input
                type="button"
                class="btn btn-default"
                data-dismiss="modal"
                value="Hủy"
              />
              <input type="submit" class="btn btn-success" value="Thêm" />
            </div>
          </form>
        </div>
      </div>
    </div>

    <!-- Edit Student Modal HTML -->
    <div id="editEmployeeModal" class="modal fade" ref="editModal">
      <div class="modal-dialog">
        <div class="modal-content">
          <form @submit.prevent="updateStudent">
            <div class="modal-header">
              <h4 class="modal-title">Sửa thông tin sinh viên</h4>
              <button
                type="button"
                class="close"
                data-dismiss="modal"
                aria-hidden="true"
              >
                &times;
              </button>
            </div>
            <div class="modal-body">
              <div class="form-group">
                <label>Tên sinh viên</label>
                <input
                  v-model="editingStudent.student_name"
                  type="text"
                  class="form-control"
                  required
                />
              </div>
              <div class="form-group">
                <label>Email</label>
                <input
                  v-model="editingStudent.email"
                  type="email"
                  class="form-control"
                  required
                />
              </div>
              <div class="form-group">
                <label>Địa chỉ</label>
                <textarea
                  v-model="editingStudent.address"
                  class="form-control"
                  required
                ></textarea>
              </div>
              <div class="form-group">
                <label>Số điện thoại</label>
                <input
                  v-model="editingStudent.phone"
                  type="text"
                  class="form-control"
                  required
                />
              </div>
            </div>
            <div class="modal-footer">
              <input
                type="button"
                class="btn btn-default"
                data-dismiss="modal"
                value="Hủy"
              />
              <input type="submit" class="btn btn-info" value="Lưu" />
            </div>
          </form>
        </div>
      </div>
    </div>

    <!-- Delete Modal HTML -->
    <div id="deleteEmployeeModal" class="modal fade" ref="deleteModal">
      <div class="modal-dialog">
        <div class="modal-content">
          <form @submit.prevent="deleteStudent">
            <div class="modal-header">
              <h4 class="modal-title">Xóa sinh viên</h4>
              <button
                type="button"
                class="close"
                data-dismiss="modal"
                aria-hidden="true"
              >
                &times;
              </button>
            </div>
            <div class="modal-body">
              <p>Bạn chắc chắn muốn xóa sinh viên này?</p>
            </div>
            <div class="modal-footer">
              <input
                type="button"
                class="btn btn-default"
                data-dismiss="modal"
                value="Hủy"
              />
              <input type="submit" class="btn btn-danger" value="Xóa" />
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed } from "vue";
import axios from "axios";

const students = ref([]);
const deleteModal = ref(null);
const addModal = ref(null);
const editModal = ref(null);
const studentIdToDelete = ref(null);
const newStudent = ref({
  student_name: "",
  email: "",
  address: "",
  phone: "",
});
const editingStudent = ref({
  id: null,
  student_name: "",
  email: "",
  address: "",
  phone: "",
});

const currentPage = ref(1);
const pageSize = 5;

const totalStudents = computed(() => students.value.length);
const totalPages = computed(() => Math.ceil(totalStudents.value / pageSize));

const paginatedStudents = computed(() => {
  const start = (currentPage.value - 1) * pageSize;
  const end = start + pageSize;
  return students.value.slice(start, end);
});

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};

const getAllStudent = async () => {
  try {
    const response = await axios.get("http://localhost:8080/students");
    students.value = response.data;
    currentPage.value = 1;
  } catch (error) {
    console.error("Lỗi khi lấy dữ liệu", error);
  }
};

const openDeleteModal = (id) => {
  studentIdToDelete.value = id;
  $(deleteModal.value).modal("show");
};

const deleteStudent = async () => {
  try {
    await axios.delete(
      `http://localhost:8080/students/${studentIdToDelete.value}`
    );
    $(deleteModal.value).modal("hide");
    getAllStudent();
  } catch (error) {
    console.error("Lỗi khi xóa dữ liệu", error);
  }
};

const validateInput = (student) => {
  if (!student.student_name || !student.email) {
    alert("Tên sinh viên và Email không được để trống");
    return false;
  }
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(student.email)) {
    alert("Email không đúng định dạng");
    return false;
  }
  if (!/^\d+$/.test(student.phone)) {
    alert("Số điện thoại chỉ được phép nhập số");
    return false;
  }
  return true;
};

const openAddModal = () => {
  newStudent.value = { student_name: "", email: "", address: "", phone: "" };
  $(addModal.value).modal("show");
};

const addStudent = async () => {
  if (!validateInput(newStudent.value)) return;

  try {
    await axios.post("http://localhost:8080/students", newStudent.value);
    $(addModal.value).modal("hide");
    getAllStudent();
  } catch (error) {
    console.error("Lỗi khi thêm sinh viên", error);
  }
};

const openEditModal = (student) => {
  editingStudent.value = { ...student };
  $(editModal.value).modal("show");
};

const updateStudent = async () => {
  if (!validateInput(editingStudent.value)) return;

  try {
    await axios.put(
      `http://localhost:8080/students/${editingStudent.value.id}`,
      editingStudent.value
    );
    $(editModal.value).modal("hide");
    getAllStudent();
  } catch (error) {
    console.error("Lỗi khi cập nhật sinh viên", error);
  }
};

onMounted(() => {
  getAllStudent();
});
</script>
