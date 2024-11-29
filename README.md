گام اول: تحلیل برنامه و یافتن نقاط نابهینگی
بر اساس نتیجه گزارشات profiling ابزار yourkit میزان زیادی از cpu در متود temp صرف شده است و همچنین از نظر استفاده از memory، متغیر محلی `Arraylist a` و به تبعش اشیا `Integer` ساخته شده برای محتویات آن بیش‌ترین میزان مصرف از حافظه را داشته‌اند. این میزان مصرف به حدی بود که سیستم کامپیوتری بنده در اجرای کامل آن دچار مشکل شد و خطای OutOfMemory خورد.
![before_fix1](https://github.com/user-attachments/assets/06204736-4924-44c3-bcd9-1ba79054ccd1)
![before_fix2](https://github.com/user-attachments/assets/94275055-528b-4090-9c09-034124ff0902)
![before_fix3](https://github.com/user-attachments/assets/a8e72411-46a9-4182-8f45-77af1a261674)
