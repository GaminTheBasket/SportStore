## ShopTheThao — ASP.NET Core MVC (.NET 8)

Ứng dụng web bán đồ thể thao sử dụng ASP.NET Core MVC, Entity Framework Core và ASP.NET Core Identity. Dự án cung cấp trang bán hàng công khai, giỏ hàng, đặt đơn và khu vực quản trị.

### Tính năng chính
- Sản phẩm: danh sách, chi tiết, tạo/sửa/xóa (qua Admin)
- Giỏ hàng: thêm/xóa sản phẩm, cập nhật số lượng, lưu trong session
- Đơn hàng: tạo đơn, xem trạng thái, theo dõi
- Quản trị: quản lý sản phẩm, khuyến mãi/giảm giá, người dùng, báo cáo doanh thu
- Xác thực/Phân quyền: ASP.NET Core Identity (đăng ký/đăng nhập, vai trò)

### Công nghệ
- ASP.NET Core 8 MVC
- Entity Framework Core (Code-First Migrations)
- ASP.NET Core Identity
- Bootstrap, jQuery (từ `wwwroot/lib`)

---

## Yêu cầu
- .NET SDK 8.x
- SQL Server (Express/LocalDB hoặc tương thích)
- Visual Studio 2022 (khuyên dùng) hoặc VS Code + C# Dev Kit

---

## Khởi động nhanh

### Cách 1: Visual Studio
1. Mở solution `ShopTheThao.sln`.
2. Đặt dự án `ShopTheThao` làm Startup Project.
3. Nhấn F5 để chạy (Debug).

### Cách 2: .NET CLI
```powershell
cd "D:\SEASON_3_ver_2\BACK-END\ShopTheThao\ShopTheThao"
dotnet restore
dotnet run
```
- Ứng dụng thường chạy tại `http://localhost:5000` và/hoặc `https://localhost:5001`
- Có thể thay đổi cổng trong `Properties/launchSettings.json`

---

## Cấu hình ứng dụng

### Chuỗi kết nối CSDL
- Mặc định cấu hình trong `ShopTheThao/appsettings.json` (mục `ConnectionStrings` nếu có).
- Ở môi trường phát triển, nên đặt secrets (chuỗi kết nối, API keys) trong `appsettings.Development.json` (không public) hoặc dùng User Secrets/biến môi trường.

### Áp dụng Migrations/khởi tạo CSDL
```powershell
cd "D:\SEASON_3_ver_2\BACK-END\ShopTheThao\ShopTheThao"
dotnet tool install --global dotnet-ef # (nếu chưa có)
dotnet ef database update
```
- Tạo migration mới khi thay đổi model:
```powershell
dotnet ef migrations add <TenMigration>
dotnet ef database update
```

---

## Tài khoản và phân quyền

- Khu vực Identity nằm tại `ShopTheThao/Areas/Identity`.
- Đường dẫn mặc định:
  - Đăng ký: `/Identity/Account/Register`
  - Đăng nhập: `/Identity/Account/Login`
- Nếu dự án có seeding tài khoản/vai trò admin, vui lòng xem logic seed trong `Program.cs` hoặc lớp seeding tương ứng. Nếu chưa có:
  - Tạo tài khoản bằng trang đăng ký
  - Cấp vai trò thủ công (qua trang quản trị người dùng nếu có, hoặc trực tiếp trong DB)

---

## Điều hướng/Routes quan trọng

- Công khai:
  - Trang chủ: `/` (controller: `HomeController`)
  - Sản phẩm: `/Products` (controller: `ProductsController`)
  - Chi tiết sản phẩm: `/Products/Details/{id}`
  - Giỏ hàng: `/Cart` (controller: `CartController`)
  - Đơn hàng: tạo/xem: `/Orders` (controller: `OrdersController`)
- Quản trị (Areas/Admin):
  - Sản phẩm: `/Admin/Products`
  - Giảm giá: `/Admin/Discounts`
  - Doanh thu: `/Admin/Revenue`
  - Người dùng: `/Admin/Users`

Lưu ý: Các trang quản trị yêu cầu đăng nhập và (thường) quyền phù hợp.

---

## Cấu trúc thư mục (rút gọn)

- `ShopTheThao/Controllers` — Controllers công khai (`Home`, `Products`, `Cart`, `Orders`)
- `ShopTheThao/Areas/Admin` — Controllers/Views khu vực quản trị (`Products`, `Discounts`, `Revenue`, `Users`)
- `ShopTheThao/Areas/Identity` — Trang Identity (đăng ký/đăng nhập/quên mật khẩu, v.v.)
- `ShopTheThao/Models` — Domain models, ViewModels (ví dụ: `Product`, `Order`, `OrderDetail`, …)
- `ShopTheThao/Data/ApplicationDbContext.cs` — DbContext chính
- `ShopTheThao/Migrations` — EF Core migrations
- `ShopTheThao/Views` — Razor Views (public)
- `ShopTheThao/wwwroot` — Static files (css/js/images)
- `ShopTheThao/Program.cs` — Cấu hình DI, middleware, routing

---

## Kịch bản thường dùng

- Chạy Debug:
```powershell
cd "D:\SEASON_3_ver_2\BACK-END\ShopTheThao\ShopTheThao"
dotnet build -c Debug
dotnet run
```

- Publish self-contained (Windows x64):
```powershell
dotnet publish -c Release -r win-x64 --self-contained true -o .\publish
```
Triển khai nội dung trong thư mục `publish` lên máy chủ (IIS, Windows Service, hoặc reverse proxy).

---

## Góp ý/Phát triển

- Issues/PRs được chào đón.
- Khi sửa đổi model, nhớ tạo migration và cập nhật DB.
- Tuân thủ chuẩn mã nguồn C# (naming, formatting) và tách nhỏ commit hợp lý.

---
