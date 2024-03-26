import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object RetrofitInstance {

    private const val BASE_URL = "http://kasimadalan.pe.hu/"

    val retrofit: Retrofit by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }

    val apiService: ApiService by lazy {
        retrofit.create(ApiService::class.java)
    }
}
import retrofit2.Call
import retrofit2.http.GET

interface ApiService {

    @GET("yemekler/tumYemekleriGetir.php")
    fun getAllMeals(): Call<MealResponse>
}
import com.google.gson.annotations.SerializedName

data class Meal(
    @SerializedName("yemek_id") val id: String,
    @SerializedName("yemek_adi") val name: String,
    @SerializedName("yemek_resim_adi") val imageUrl: String,
    @SerializedName("yemek_fiyat") val price: String
)

data class MealResponse(
    @SerializedName("yemekler") val meals: List<Meal>,
    @SerializedName("success") val success: Int
)
import android.os.Bundle
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.recyclerview.widget.LinearLayoutManager
import kotlinx.android.synthetic.main.activity_main.*
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response

class MainActivity : AppCompatActivity() {

    private lateinit var mealAdapter: MealAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        mealAdapter = MealAdapter()

        recyclerView.apply {
            layoutManager = LinearLayoutManager(this@MainActivity)
            adapter = mealAdapter
        }

        // API'den tüm yemekleri alma ve RecyclerView'a ekleme
        fetchAllMeals()
    }

    private fun fetchAllMeals() {
        val call = RetrofitInstance.apiService.getAllMeals()
        call.enqueue(object : Callback<MealResponse> {
            override fun onResponse(call: Call<MealResponse>, response: Response<MealResponse>) {
                if (response.isSuccessful) {
                    response.body()?.let { mealResponse ->
                        if (mealResponse.success == 1) {
                            mealAdapter.submitList(mealResponse.meals)
                        } else {
                            showToast("Yemekler getirilemedi.")
                        }
                    }
                } else {
                    showToast("Yemekler getirilemedi.")
                }
            }

            override fun onFailure(call: Call<MealResponse>, t: Throwable) {
                showToast("Bağlantı hatası: ${t.message}")
            }
        })
    }

    private fun showToast(message: String) {
        Toast.makeText(this@MainActivity, message, Toast.LENGTH_SHORT).show()
    }
}

