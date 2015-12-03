# sqlhelper

public class SqlHelper
    {

        private static readonly string connstr = ConfigurationManager.ConnectionStrings["sql"].ConnectionString;
        public static int ExecuteNonQuery(string sql, CommandType cmdType, params SqlParameter[] ps)
        {
            using (SqlConnection conn = new SqlConnection(connstr))
            {
                using (SqlCommand cmd = new SqlCommand(sql, conn))
                {
                    cmd.CommandType = cmdType;
                    if (ps != null)
                    {
                        cmd.Parameters.AddRange(ps);
                    }
                    conn.Open();
                    return cmd.ExecuteNonQuery();
                }
            }
        }

        public static object ExecuteScale(string sql, CommandType cmdType, params SqlParameter[] ps)
        {
            using (SqlConnection conn = new SqlConnection(connstr))
            {
                using (SqlCommand cmd = new SqlCommand(sql, conn))
                {
                    cmd.CommandType = cmdType;
                    if (ps != null)
                    {
                        cmd.Parameters.AddRange(ps);

                    }
                    conn.Open();
                    return cmd.ExecuteScalar();
                }
            }
        }

        public static SqlDataReader ExecuteReader(string sql, CommandType cmdType, params SqlParameter[] ps)
        {
            SqlConnection conn = new SqlConnection(connstr);

            using (SqlCommand cmd = new SqlCommand(sql, conn))
            {
                cmd.CommandType = cmdType;
                if (ps != null)
                {
                    cmd.Parameters.AddRange(ps);
                }
                try
                {
                    if (conn.State == ConnectionState.Closed)
                    {
                        conn.Open();

                    }
                    return cmd.ExecuteReader(CommandBehavior.CloseConnection);
                }
                catch (Exception ex)
                {
                    conn.Close();
                    conn.Dispose();
                    throw ex;

                }
            }

        }

        public static DataTable ExecuteDataTable(string sql, CommandType cmdType, params SqlParameter[] ps)
        {
            DataTable dt = new DataTable();

            using (SqlDataAdapter adapter = new SqlDataAdapter(sql, connstr))
            {
                adapter.SelectCommand.CommandType = cmdType;
                if (ps != null)
                {
                    adapter.SelectCommand.Parameters.AddRange(ps);
                }
                adapter.Fill(dt);



            }

            return dt;



        }

    }
